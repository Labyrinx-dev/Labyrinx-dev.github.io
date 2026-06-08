---
layout: default
---

# Labyrinx — Python Code Protection

{: .text-center}
**Turn Python projects into protected, standalone Windows executables. Seven layers of obfuscation. Proprietary EXE format. Zero-trace execution.**

---

## What Labyrinx Does

Labyrinx obfuscates your Python source code and packages it into a single `.exe` using the **LBRX Native Packager** — a proprietary, encrypted container format resistant to common extraction tools. No Python installation required on the target machine.

| Layer | Protection | Level |
|-------|-----------|-------|
| 1. Name Obfuscation | Variables, functions, and classes renamed to random identifiers | 2 |
| 2. String Encryption | Every string literal packed into an encrypted blob | 3 |
| 3. Control Flow Flattening | Function logic restructured into switch-case state machines | 4 |
| 4. Dead Code Injection | Opaque predicates intersperse junk blocks between real statements | 4 |
| 5. Anti-Debug | Multiple debugger detection techniques covering both user-mode and kernel-mode approaches | 5 |
| 6. Module Encryption | Entire `.py` modules AES-256 encrypted — decrypted only in memory at runtime | 5 |
| 7. Code Virtualization | Custom stack-based VM replaces Python bytecode — 49 opcodes with encrypted constants | 6 |

{: .text-center}
*No other Python obfuscator ships a code VM.*

---

## LBRX Native Packager

Unlike standard packagers, Labyrinx uses a custom native bootloader and a fully encrypted container format. Your code, dependencies, and runtime are bundled into a single `.exe`:

| Feature | Detail |
|---------|--------|
| **Per-EXE encryption** | Every build gets a unique random key — no two EXEs are alike |
| **Tamper detection** | Cryptographic integrity verification prevents modification to the packaged application |
| **Runtime hardening** | Native components verified before loading — replacement is detected and blocked |
| **Zero-trace execution** | All temporary files securely wiped after the application exits |
| **Smart compression** | Text assets compressed automatically, saving significant payload size |
| **Deep dependency resolution** | AST-based recursive import scanner resolves transitive dependencies automatically |
| **Dual-mode support** | Build EXEs with or without a console window — non-console builds show an extraction progress display |

{: .text-center}
*Two packagers included: LBRX Native (proprietary, recommended) and the standard packager.*

---

## Proven Across the Python Ecosystem

The LBRX Native Packager has been stress-tested against a broad range of real-world Python projects spanning **web frameworks, ORMs, data science libraries, CLI tools, imaging libraries, PDF generators, and cryptographic toolkits**. All pass at 100%.

### Tamper Resistance — All Attacks Blocked

Multiple tampering vectors were tested against the bootloader. Every attempt was detected and blocked before any code execution occurred.

### Protection Scaling

| Level | Startup | Features Active |
|-------|---------|-----------------|
| 1 | Instant | Base obfuscation |
| 2 | Instant | + Name obfuscation |
| 3 | Instant | + String encryption |
| 4 | Instant | + Control flow flattening |
| 5 | Fast | + Anti-debug + Module encryption |
| 6 | Fast | + 49-opcode code VM |

{: .small}
*Measured on a typical mid-range Windows workstation. Actual times vary by project.*

---

## How It Works

```
Your .py files  -->  Labyrinx (7 layers)  -->  Protected .exe
                                           -->  Standalone, no Python needed
```

1. **Open Labyrinx** — point it at your Python project or single `.py` file
2. **Choose protection level** — 1 (base) through 6 (full VM + anti-debug + AES-256)
3. **Click Build** — Labyrinx obfuscates every `.py` file, resolves all dependencies automatically, and packages an `.exe`
4. **Distribute** — single `.exe` with optional built-in license key system and installer builder

{: .text-center}
*CLI and GUI both included. GUI for interactive use, CLI for CI/CD pipelines.*

---

## Pricing

| | Freemium | Pro | Enterprise |
|---|---------|-----|------------|
| **Price** | Free | $9/month | $29/month |
| **Max Level** | 2 | 4 | 6 |
| **String Encryption** | -- | Yes | Yes |
| **Module Encryption** | -- | -- | Yes |
| **Anti-Debug** | -- | -- | Yes |
| **Code VM** | -- | -- | Yes |
| **License System** | -- | Yes | Yes |
| **Installer Builder** | -- | Yes | Yes |
| **LBRX Native Packager** | -- | Yes | Yes |
| **Per-EXE Keys** | -- | -- | Yes |
| **Zero-Trace Cleanup** | -- | -- | Yes |

{: .text-center}
*All tiers include lifetime access. No per-build fees. No royalties.*

{: .text-center}
[**Subscribe Now**](https://labyrinx.lemonsqueezy.com) — instant license key delivery after payment.

---

## FAQ

**Q: Does it work with major Python libraries and frameworks?**
A: Yes. Labyrinx has been stress-tested against a wide range of real-world libraries — web frameworks, ORMs, data science toolkits, CLI applications, imaging libraries, PDF generators, and cryptographic libraries. All pass at 100%. The deep import scanner automatically resolves multi-level transitive dependencies.

**Q: Does the protected EXE need Python installed?**
A: No. The EXE bundles the complete Python runtime — fully standalone.

**Q: Can my EXE be unpacked?**
A: Not with the LBRX Native packager. Unlike standard packagers that store code in plain formats, LBRX uses per-EXE encryption, cryptographic integrity verification, and an encrypted file table. Common extraction tools cannot parse the proprietary format.

**Q: Is my source code recoverable at Level 6?**
A: At Level 6, the original Python bytecode is entirely replaced with custom VM bytecode. Even if the encrypted payload were somehow accessed, what's inside is a stream of VM opcodes with no mapping back to original Python source.

**Q: Can I sell software protected with Labyrinx?**
A: Yes. The built-in license key system lets you generate and distribute license keys to your customers. An installer builder is also included.

**Q: What is the minimum EXE size?**
A: Approximately 29 MB for a minimal application (includes the Python runtime, standard library, and Labyrinx runtime). Larger projects scale primarily with their dependencies.

**Q: What Windows versions are supported?**
A: Windows 10 and later, 64-bit.

---

{: .text-center}
## Downloads

| Tool | Description | Size |
|------|-------------|------|
| **Labyrinx GUI** | Point-and-click obfuscation and EXE builder | 125 MB |
| **Labyrinx CLI** | Command-line for CI/CD and automation | 125 MB |
| **HWID Tool** | Customers run this to get their machine ID | 10 MB |
| **License Creator** | Generate license keys for your customers | 10 MB |
| **User Manual** | Full documentation and CLI reference | 23 KB |

{: .text-center}
[**Browse All Downloads**](https://github.com/Labyrinx-dev/Labyrinx-dev.github.io/tree/master/release)

{: .small}
*System requirements: Windows 10+, 64-bit. No Python installation needed.*<br>
*For direct download links and trial licenses, [contact us](#contact).*

---

{: #contact}
## Contact & Sales

{: .text-center}
For trial requests, licensing inquiries, and enterprise pricing:

{: .text-center}
📧 **labyrinx@yahoo.com**

{: .text-center}
🐙 **[github.com/Labyrinx-dev](https://github.com/Labyrinx-dev)**

---

{: .text-center .small}
(c) 2026 Labyrinx. All rights reserved.
