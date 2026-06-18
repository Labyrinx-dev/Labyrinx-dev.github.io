---
layout: default
title: How to Protect Python Source Code — A Complete Guide
seo_description: Learn how to protect Python source code from reverse engineering. Covers name obfuscation, Cython compilation, AES string encryption, and code virtualization. Honest about what each protection layer can and cannot do.
---

# How to Protect Python Source Code from Reverse Engineering

{: .text-center}
*Python ships as readable text. Here's how to change that — from basic obfuscation to custom virtual machines.*

---

## The Problem

Python is distributed as source code. Even when you bundle it with PyInstaller, `pyinstxtractor` extracts the `.pyc` bytecode, and `pycdc` / `uncompyle6` decompile it back to readable Python in seconds.

```bash
# An attacker extracts your entire source in under a minute:
> pyinstxtractor your_app.exe
> uncompyle6 extracted/your_code.pyc
# Output: perfectly readable Python. Variable names, strings, comments — everything.
```

This isn't a Python flaw — it's inherent to how interpreted languages work. Protection means adding layers that each make reverse engineering more expensive.

---

## The Four Levels of Protection

### Level 1: Obfuscation

**What it does:** Makes the code harder for humans to read.

**Techniques:**
- **Name obfuscation** — rename all variables, functions, and classes to meaningless tokens (`calculate_total()` → `_x7f3a()`). Makes the code functionally correct but unreadable.
- **String encoding** — Base64 or XOR simple string literals so `strings.exe` doesn't immediately reveal API keys, URLs, and error messages.
- **Comment/docstring stripping** — remove all comments and docstrings. They're documentation for humans; attackers use them as roadmaps.

**What it stops:** Casual reading. A curious developer opening your `.py` file.
**What it doesn't stop:** Anyone with a debugger. Names are cosmetic.

### Level 2: Compilation (Cython / Nuitka)

**What it does:** Converts Python to native x64 machine code.

Unlike PyInstaller (which just bundles `.pyc` bytecode), Cython and Nuitka translate Python to C, then compile with MSVC or GCC. The output is a `.pyd` (Windows) or `.so` (Linux) file — a native shared library.

```
.py source → Cython → .c file → MSVC/GCC → .pyd (x64 machine code)
```

**What it stops:** Decompilation. There's no bytecode to decompile. An attacker gets assembly, not Python.
**What it doesn't stop:** `strings.exe` still finds every string literal. An experienced reverse engineer can trace the assembly back to the original logic with enough time.

### Level 3: Encryption

**What it does:** Encrypts the sensitive parts of your code so they don't exist in plaintext anywhere.

**Techniques:**
- **String encryption (AES-256)** — every string literal is replaced with `decrypt(encrypted_blob, key)`. The key is embedded in the compiled `.pyd` native code, not a separate file. `strings.exe` finds nothing.
- **Module encryption** — entire Python modules are AES-256 encrypted blobs. They're only decrypted into memory at import time. The source file on disk is opaque ciphertext.

**What it stops:** Static analysis tools. `strings.exe`. Anyone peeking at files on disk.
**What it doesn't stop:** Memory dumps. When the module is loaded, the decrypted content exists in RAM. A sophisticated attacker can capture it there.

### Level 4: Virtualization

**What it does:** Replaces Python bytecode with a **custom bytecode** that only a custom virtual machine understands.

Instead of `import my_module` loading standard Python bytecode, it loads instructions for a VM that Labyrinx built. The VM's instruction set is **randomized per build** — the opcode for "add" in build #1 is `0x4F`; in build #2 it's `0x93`. An attacker who reverses one build learns nothing about the next.

```
Standard Python:  .py → standard bytecode → Python VM
Labyrinx VM:       .py → Cython .pyd → custom VM bytecode → custom VM interpreter
```

**What it stops:** All standard Python reverse engineering tools. The bytecode format is unknown.
**What it doesn't stop:** A dedicated reverse engineer who traces the VM interpreter itself and reconstructs the instruction set.

---

## What No Tool Can Do

Let's be honest about the limits:

| Claim | Reality |
|-------|---------|
| "Unbreakable protection" | Nothing is unbreakable. Given enough time and skill, any code can be reversed. |
| "Hide all logic" | The CPU must execute the logic. A debugger attached to the process can trace it. |
| "Prevent piracy" | Protection makes copying harder, not impossible. License keys + HWID binding help, but determined attackers find ways. |
| "Zero performance impact" | Obfuscation and encryption add overhead. VM-based protection has the most. |

The realistic goal is not "unbreakable." It's **"more expensive to break than to build."**

---

## The Labyrinx Approach: All Four Levels

Labyrinx chains all four levels in a single build pipeline:

```
1. Source code (.py)
     │
2. AST Transform → Name obfuscation, control flow flattening
     │
3. Cython → .c → MSVC → .pyd (native x64 code)
     │
4. String encryption → AES-256 encrypt all string literals
     │
5. Module encryption → AES-256 encrypt the module payload
     │
6. VM pass → Convert critical code to custom VM bytecode
     │
7. Anti-debug → Embed debugger detection at multiple points
     │
8. Output folder → python313.dll + .pyd files + tiny launcher
```

Each layer independently raises the cost of reverse engineering. Together, they make it **prohibitively expensive** for most attackers.

---

## Getting Started

**Immediate (free):** Use [`pyobfuscate`](https://pypi.org/project/pyobfuscate/) or [`pyminifier`](https://pypi.org/project/pyminifier/) to rename identifiers and strip comments. It's better than nothing and takes 10 seconds.

**Next step (free):** Install [Cython](https://cython.org/) and compile your core modules to `.pyd`. Even without obfuscation, native code is far harder to reverse than bytecode. Requires a C compiler (MSVC on Windows, GCC on Linux).

**Full protection:** [Labyrinx](/compare/) automates all four levels. One-click build, output folder ships to customers. Freemium tier is free; Enterprise unlocks the full 7-layer pipeline.

---

{: .text-center}
[← Back to Labyrinx](/) &nbsp;·&nbsp; [Python Code Protection Tools Compared →](/compare/)
