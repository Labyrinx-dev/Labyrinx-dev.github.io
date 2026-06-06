---
layout: default
---

# Labyrinx — Python Code Protection

{: .text-center}
**Turn Python projects into protected, standalone Windows executables. 7 layers of obfuscation. Proprietary EXE format. Zero-trace execution.**

---

## What Labyrinx Does

Labyrinx obfuscates your Python source code and packages it into a single `.exe` using the **LBRX Native Packager** — a proprietary, AES-256-CTR encrypted format that is resistant to extraction tools like `pyinstxtractor`. No Python installation required on the target machine.

| Layer | Protection | Level |
|-------|-----------|-------|
| 1. Name Obfuscation | Variables, functions, classes renamed to random identifiers | 2 |
| 2. String Encryption | Every string literal packed into encrypted blobs (XOR + base85) | 3 |
| 3. Control Flow Flattening | Function logic flattened into switch-case state machines | 4 |
| 4. Dead Code Injection | Opaque predicates inject junk code blocks between real statements | 4 |
| 5. Anti-Debug (x6) | `gettrace`, `IsDebuggerPresent`, `DebugPort`, `NtGlobalFlag`, timing, hardware breakpoints | 5 |
| 6. Module Encryption | AES-256-CTR encrypts entire `.py` modules — decrypted only in memory at runtime | 5 |
| 7. Code Virtualization | 49-opcode custom stack VM replaces Python bytecode entirely | 6 |

{: .text-center}
*No other Python obfuscator ships a code VM.*

---

## LBRX Native Packager — How the EXE Works

Unlike PyInstaller-based packagers, Labyrinx uses a custom C bootloader and a proprietary, fully encrypted container format. Your code, dependencies, and runtime files are bundled into a single `.exe` with military-grade protection:

| Feature | Detail |
|---------|--------|
| **AES-256 encryption** | Every EXE gets a unique random key — no two builds are alike |
| **Tamper-proof** | Cryptographic integrity verification prevents any modification to the packaged EXE |
| **Runtime hardening** | All `.pyd` native modules verified before loading — replacement is detected and blocked |
| **Zero-trace execution** | All extracted files are securely wiped after the application exits — nothing left on disk |
| **Smart compression** | Text assets compressed automatically, saving 30-45% on payload size |
| **Deep import scan** | AST-based recursive dependency resolution — specify `reportlab` and Labyrinx automatically pulls the full chain: `rlPyCairo` → `pycairo` → `cairo` + `Pillow` → `PIL` + `freetype` |
| **Standard crypto** | Uses Windows built-in cryptographic providers — no hand-rolled algorithms |

{: .text-center}
*Two packagers included: LBRX Native (proprietary, recommended) and PyInstaller (standard).*

---

## Proven — Stress Test Results

The LBRX v9 native packager has been thoroughly tested against real open-source Python projects. **100% pass rate across 27 tests in 6 categories.**

### Real Projects — All Pass at Level 2 Obfuscation

| Project | EXE Size | Build Time | Run Time |
|---------|----------|-----------|----------|
| **yt-dlp** 2026.3.17 | 70.2 MB | 75s | 17.9s |
| **Flask** 3.1.3 | 52.5 MB | 40s | 9.6s |
| **Pygments** 2.20.0 | 34.3 MB | 25s | 6.7s |
| **SQLAlchemy** 2.0.49 | 40.7 MB | 35s | 8.4s |
| **Pillow** 12.2.0 | 75.3 MB | 47s | 9.3s |
| **pandas** 3.0.3 | 190.2 MB | 151s | 26.2s |
| **cryptography** 48.0.0 | 42.7 MB | 28s | 6.9s |
| **reportlab** | 87.7 MB | 60s | 11.6s |

### Bootloader Integrity — All Tampering Detected

| Attack | Result |
|--------|--------|
| Footer magic corruption | `FAIL:magic` — exit 1 |
| Payload byte flip | `FAIL:integrity` — exit 1 |
| Footer truncation | `FAIL:magic` — exit 1 |
| Key recovery tamper | `FAIL:integrity` — exit 1 |

### Obfuscation Level Scaling

| Level | EXE Size | Startup | Features Active |
|-------|----------|---------|-----------------|
| 1 | 28.8 MB | 5.0s | Base obfuscation |
| 2 | 28.8 MB | 4.9s | + Name obfuscation |
| 3 | 28.9 MB | 4.9s | + String encryption |
| 4 | 28.9 MB | 5.0s | + Control flow flattening |
| 5 | 42.7 MB | 15.4s | + Anti-debug + Module AES-256 |
| 6 | 42.7 MB | 14.7s | + 49-opcode VM |

---

## How It Works

```
Your .py files  -->  Labyrinx (7 layers)  -->  Protected .exe
                                           -->  Standalone, no Python needed
```

1. **Open Labyrinx** — point it at your Python project or single `.py` file
2. **Choose protection level** — 1 (base) through 6 (full VM + anti-debug + AES-256 module encryption)
3. **Click Build** — Labyrinx obfuscates every `.py` file, resolves all dependencies automatically, and packages an `.exe`
4. **Distribute** — single `.exe` with optional built-in license key system and installer builder

{: .text-center}
*CLI and GUI both included. GUI for interactive use, CLI for CI/CD pipelines.*

---

## Pricing

| | Freemium | Pro | Enterprise |
|---|---------|-----|------------|
| **Price** | Free | $9/month | $29/month |
| **Max Level** | 2 (names) | 4 (strings, CFG, dead code) | 6 (VM, anti-debug, AES-256) |
| **EXE Bundling** | Single-file | Full project | Full project |
| **Packager** | PyInstaller | PyInstaller + LBRX Native | LBRX Native (proprietary) |
| **String Encryption** | -- | Yes | Yes |
| **Module Encryption** | -- | -- | Yes (AES-256-CTR) |
| **Anti-Debug** | -- | -- | Yes (x6 techniques) |
| **Code VM** | -- | -- | Yes (49-opcode) |
| **License System** | -- | Yes | Yes |
| **Installer Builder** | -- | Yes | Yes |
| **Per-EXE Keys** | -- | -- | Yes (LBRX Native) |
| **Zero-Trace Cleanup** | -- | -- | Yes (LBRX Native) |

{: .text-center}
*All tiers include lifetime access. No per-build fees. No royalties.*

---

## FAQ

**Q: Does it work with Flask, pandas, SQLAlchemy, yt-dlp, and other real-world libraries?**
A: Yes. Labyrinx has been stress-tested against yt-dlp, Flask, Pygments, SQLAlchemy, Pillow, pandas, cryptography, and reportlab — all pass at 100%. The deep import scanner automatically resolves transitive dependencies (e.g., `reportlab` alone pulls in `rlPyCairo`, `pycairo`, `cairo`, `Pillow`, `PIL`, `freetype`).

**Q: Does the protected EXE need Python installed?**
A: No. The EXE bundles the full Python 3.13 runtime — completely standalone.

**Q: Can my EXE be unpacked with pyinstxtractor or similar tools?**
A: Not with the LBRX Native packager. Unlike PyInstaller (which stores plain `.pyc` files in a ZIP overlay), LBRX uses AES-256-CTR with per-EXE random keys, SHA-256 integrity verification, and encrypted file tables. Standard extraction tools cannot parse the proprietary format.

**Q: Is my source code still readable at Level 6?**
A: At Level 6, the original Python bytecode is entirely replaced with custom 49-opcode VM bytecode. Even if the encrypted payload is somehow decrypted and extracted, an attacker sees:
```
PUSH_CONST 142  ->  PUSH_FAST 3  ->  BINOP_ADD  ->  CALL 2  ->  ...
```
There is no mapping back to original Python source.

**Q: Can I sell software protected with Labyrinx?**
A: Yes. The built-in license system (HMAC-based) lets you generate and distribute license keys to your customers. An installer builder is also included.

**Q: What is the minimum EXE size?**
A: Approximately 29 MB for a hello-world application (includes the Python 3.13 runtime, standard library, and Labyrinx `.pyd` runtime modules). Larger projects scale primarily with their dependencies — pandas adds ~160 MB for its native extensions.

**Q: What Windows versions are supported?**
A: Windows 10 and later, 64-bit. The bootloader uses Windows BCrypt (Vista+) for SHA-256 and RtlDecompressBuffer (XP+) for LZNT1 decompression, but the packaged Python 3.13 runtime requires Windows 10+.

---

{: .text-center .small}
(c) 2026 Labyrinx. All rights reserved.
