---
layout: default
title: How to Protect Python Source Code — A Complete Guide
seo_description: Learn how to protect Python source code from reverse engineering. Covers name obfuscation, native code compilation, AES string encryption, and code virtualization. Honest about what each protection layer can and cannot do.
---

# How to Protect Python Source Code from Reverse Engineering

{: .text-center}
*Python ships as readable text. Here's how to change that — from basic obfuscation to custom virtual machines.*

---

## The Problem

Python is distributed as source code. Even when bundled into an EXE, well-known extraction tools recover the `.pyc` bytecode, and decompilers turn it back into readable Python in seconds.

```bash
# An attacker extracts your source in under a minute:
> extractor your_app.exe
> decompiler extracted/your_code.pyc
# Output: readable Python. Variable names, strings, comments — everything.
```

This isn't a Python flaw — it's inherent to how interpreted languages work. Protection means adding layers that each make reverse engineering more expensive.

---

## The Four Levels of Protection

### Level 1: Obfuscation

**What it does:** Makes the code harder for humans to read.

**Techniques:**
- **Name obfuscation** — rename all variables, functions, and classes to meaningless tokens. Makes the code functionally correct but unreadable.
- **String encoding** — encode string literals so string extraction tools don't immediately reveal API keys, URLs, and error messages.
- **Comment/docstring stripping** — remove all comments and docstrings that serve as roadmaps for attackers.

**What it stops:** Casual reading. A curious developer opening your `.py` file.
**What it doesn't stop:** Anyone with a debugger. Names are cosmetic.

### Level 2: Native Code Compilation

**What it does:** Converts Python to native x64 machine code.

Unlike EXE bundling (which just packages `.pyc` bytecode), native compilation translates Python to C, then compiles to a `.pyd` (Windows) or `.so` (Linux) file — a native shared library.

```
.py source → C translation → .c file → C compiler → .pyd (x64 machine code)
```

**What it stops:** Decompilation. There's no bytecode to decompile. An attacker gets assembly, not Python.
**What it doesn't stop:** String extraction tools still find every string literal. An experienced reverse engineer can trace the assembly back to the original logic with enough time.

### Level 3: Encryption

**What it does:** Encrypts the sensitive parts of your code so they don't exist in plaintext anywhere.

**Techniques:**
- **String encryption (AES-256)** — every string literal is replaced with an encrypted blob decoded at runtime. String extraction tools find nothing.
- **Module encryption** — entire Python modules are AES-256 encrypted blobs decrypted into memory at import time. The source file on disk is opaque ciphertext.

**What it stops:** Static analysis tools, string extraction, anyone peeking at files on disk.
**What it doesn't stop:** Memory dumps. When the module is loaded, the decrypted content exists in RAM.

### Level 4: Virtualization

**What it does:** Replaces Python bytecode with a **custom bytecode** that only a custom virtual machine understands.

The VM's instruction set is **randomized per build** — the opcode for "add" in build #1 is `0x4F`; in build #2 it's `0x93`. An attacker who reverses one build learns nothing about the next.

**What it stops:** All standard Python reverse engineering tools. The bytecode format is unknown.
**What it doesn't stop:** A dedicated reverse engineer who traces the VM interpreter itself.

---

## What No Tool Can Do

| Claim | Reality |
|-------|---------|
| "Unbreakable protection" | Nothing is unbreakable. Any code can be reversed. |
| "Hide all logic" | The CPU must execute the logic. A debugger can trace it. |
| "Prevent piracy" | Protection makes copying harder, not impossible. |
| "Zero performance impact" | Obfuscation and encryption add overhead. |

The realistic goal: **more expensive to break than to build.**

---

## The Labyrinx Approach: All Four Levels

Labyrinx chains all four levels in a single build pipeline:

```
1. AST Transform → Name obfuscation, control flow flattening
2. String encryption → AES-256 encrypt all string literals
3. Module encryption → AES-256 encrypt the module payload
4. Native compilation → .c → MSVC → .pyd (x64 machine code)
5. VM pass → Convert critical code to custom VM bytecode
6. Anti-debug → Embed debugger detection at multiple points
7. Output folder → python313.dll + .pyd files + launcher
```

Each layer independently raises the cost of reverse engineering. Together, they make it **prohibitively expensive** for most attackers.

---

## Getting Started

**Immediate (free):** Use name obfuscation and comment stripping. Better than nothing and takes seconds.

**Next step (free):** Use a C-to-native compiler to compile your core modules to `.pyd`. Even without obfuscation, native code is far harder to reverse than bytecode. Requires a C compiler (MSVC on Windows).

**Full protection:** [Labyrinx](/) automates all four levels. One-click build, output folder ships to customers. Freemium tier is free; Enterprise unlocks the full 7-layer pipeline.

---

{: .text-center}
[← Back to Labyrinx](/) &nbsp;·&nbsp; [Python Code Protection Approaches Compared →](/compare)
