---
layout: default
---

# Labyrinx — Python Code Protection

{: .text-center}
**Turn Python projects into protected, self-contained application folders. Seven layers of obfuscation. Native x64 compilation via Cython. No Python install required on the target machine.**

---

## What Labyrinx Does

Labyrinx obfuscates your Python source code, compiles it to native x64 machine code via Cython, and assembles a self-contained folder your customers can run immediately. No EXE packing — just clean, AV-friendly distribution.

| Layer | Protection | Tier |
|-------|-----------|------|
| 1. Name Obfuscation | Variables, functions, and classes renamed to random tokens | Freemium |
| 2. Control Flow Flattening | Function logic restructured into switch-case state machines | Pro |
| 3. String Encryption | Every string literal AES-encrypted — invisible to `strings.exe` | Pro |
| 4. Module Encryption (gzip) | Entire source compressed into a blob — decompile → gzip stream | Pro |
| 5. Module Encryption (AES-256) | Entire source AES-256-CTR encrypted — decompile → opaque ciphertext | Enterprise |
| 6. Anti-Debug | 6 detection techniques covering user-mode and kernel-mode debuggers | Enterprise |
| 7. Code Virtualization | Custom 49-opcode stack VM with per-build randomized bytecode | Enterprise |

{: .text-center}
*All tiers produce native x64 .pyd files via Cython. Pro adds gzip module encryption. Enterprise adds AES-256 + VM.*

---

## Output: Embedded Python Folder

Labyrinx produces a **self-contained folder** — not a single EXE. This approach:

- **Avoids antivirus false positives** — no EXE packing, no shellcode
- **Starts instantly** — no temp extraction, no decryption overhead
- **Supports incremental updates** — replace individual `.pyd` files
- **AV-friendly** — plain files, no obfuscated packer signatures

```
MyApp/
├── MyApp.exe              ← click-to-run (~30 KB)
├── MyApp.bat              ← fallback launcher
├── python313.dll          ← embedded Python (no install needed)
├── python313.zip          ← compressed stdlib
├── *.pyd                  ← your code (Cython → native x64)
├── _lx_*.pyd              ← Labyrinx runtime (encryption, VM)
└── Lib/site-packages/     ← third-party deps
```

{: .text-center}
*Zip the folder and ship it. Customers extract and double-click.*

---

## Real Build Performance

Tested on a real Flask application with 20 modules, 24 compiled files, ~40 Python packages:

| Tier | Level | Build Time | Output Size |
|------|-------|-----------|-------------|
| **Freemium** | 2 | ~40 seconds | ~151 MB |
| **Pro** | 4 | ~75 seconds | ~140 MB |
| **Enterprise** | 6 | ~90 seconds | ~153 MB |

{: .small}
*Windows 10 x64, Python 3.13, Cython + MSVC. All builds produce fully functional applications.*

---

## Module Encryption Comparison

| | Pro (gzip) | Enterprise (AES-256-CTR) |
|---|---|---|
| **Mechanism** | Gzip compression + base64 | AES-256-CTR encryption |
| **Build speed** | Fast (~75s) | Moderate (~90s) |
| **Runtime deps** | None (stdlib only) | 4 native .pyd files |
| **After .pyd decompilation** | Gzip stream visible | Opaque ciphertext |
| **Price** | $9/month | $29/month |

Both wrap the obfuscated source so Cython compiles a trivial 2-line stub — making builds 4-6× faster than compiling raw obfuscated Python. The difference is whether the blob is compressed or encrypted.

---

## Pricing

| | Freemium | Pro | Enterprise |
|---|---------|-----|------------|
| **Price** | Free | $9/month | $29/month |
| **Max Level** | 2 | 4 | 6 |
| **String Encryption (AES)** | — | Yes | Yes |
| **Module Encryption (gzip)** | — | Yes | — |
| **Module Encryption (AES-256)** | — | — | Yes |
| **Code VM** | — | — | Yes |
| **Anti-Debug** | — | — | Yes |
| **License System** | — | Yes | Yes |
| **Per-build Randomized PYDs** | — | — | Yes |
| **PYD Integrity Hashes** | — | — | Yes |

{: .text-center}
*All tiers include GUI + CLI. No per-build fees. No royalties.*

---

## Get Started

### 1. Download (Free)

`Labyrinx.exe` runs in **Freemium** mode out of the box — name obfuscation + native x64 .pyd output. No registration required.

{: .text-center}
[**Download Labyrinx**](https://github.com/Labyrinx-dev/Labyrinx-dev.github.io/raw/master/release/Labyrinx.exe)

{: .small}
*Includes GUI + CLI + User Manual. Windows 10+, 64-bit. Requires Cython + MSVC for .pyd compilation.*

### 2. Subscribe to Upgrade

Unlock Pro or Enterprise features instantly. License key delivered automatically.

{: .text-center}
[**Subscribe — Pro ($9/mo)**](https://labyrinx.lemonsqueezy.com/checkout/buy/d9731dc3-df7c-441c-b308-310696483a92) — Levels 1–4, gzip module encryption, string encryption  
[**Subscribe — Enterprise ($29/mo)**](https://labyrinx.lemonsqueezy.com/checkout/buy/d97c655e-ebd8-4370-b868-158498d220d3) — Level 6 VM, AES-256 module encryption, anti-debug, per-build randomized PYDs

### 3. Activate

Enter your license key in Labyrinx to unlock. Valid for the duration of your subscription. Cancel anytime.

---

## FAQ

**Q: Does the output need Python installed?**
A: No. The output folder contains a full embedded Python runtime (python313.dll + stdlib). Your customers just double-click.

**Q: Why folders instead of a single EXE?**
A: Folders avoid antivirus false positives (no EXE packing), start instantly (no temp extraction), and support incremental updates (swap individual .pyd files).

**Q: What's the difference between gzip and AES module encryption?**
A: Gzip (Pro) compresses your obfuscated source into a blob. AES (Enterprise) encrypts it with AES-256-CTR. Both make Cython compilation nearly instant. AES adds cryptographic-grade protection — after .pyd decompilation, all an attacker sees is encrypted ciphertext.

**Q: Can I sell software protected with Labyrinx?**
A: Yes. The built-in license key system (Enterprise) lets you generate and distribute signed license keys to your customers. Optional hardware binding, expiry dates, and tier enforcement.

**Q: What is the output size?**
A: Approximately 40 MB for a minimal app, 145 MB for a typical Flask web app, and 200-400 MB for data science applications. The Python runtime and standard library account for the base footprint.

**Q: What Windows versions are supported?**
A: Windows 10 and later, 64-bit.

---

{: .text-center}
## All Downloads

[Browse the release folder](https://github.com/Labyrinx-dev/Labyrinx-dev.github.io/tree/master/release) — `Labyrinx.exe` and the User Manual.  
*HWID Tool and License Creator are built from within Labyrinx (Admin panel).*

{: .text-center}
[**Browse All Downloads**](https://github.com/Labyrinx-dev/Labyrinx-dev.github.io/tree/master/release)

{: .small}
*System requirements: Windows 10+, 64-bit. Cython + MSVC required for .pyd compilation. No Python needed on target machine.*<br>
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
