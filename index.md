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
| 2. Control Flow Flattening | Function logic restructured into non-linear execution paths | Pro |
| 3. String Encryption | Every string literal encrypted — invisible to `strings.exe` | Pro |
| 4. Module Encryption (compression) | Entire source protected in a wrapped blob | Pro |
| 5. Module Encryption (AES-256) | Entire source AES-256 encrypted — decompilation yields opaque ciphertext | Enterprise |
| 6. Anti-Debug | Multiple detection techniques across multiple layers | Enterprise |
| 7. Code Virtualization | Custom VM with per-build randomized instruction set | Enterprise |

{: .text-center}
*All tiers produce native x64 .pyd files via Cython. Pro adds module encryption. Enterprise adds AES-256 + VM.*

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
├── Labyrinx runtime        ← protection modules (encryption, VM, integrity)
└── Lib/site-packages/     ← third-party deps
```

{: .text-center}
*Zip the folder and ship it. Customers extract and double-click.*

---

## Pricing

| | Freemium | Pro | Enterprise |
|---|---------|-----|------------|
| **Price** | Free | $9/month | $29/month |
| **Max Level** | 2 | 4 | 6 |
| **String Encryption (AES)** | — | Yes | Yes |
| **Module Encryption** | — | Yes | — |
| **Module Encryption (AES-256)** | — | — | Yes |
| **Code VM** | — | — | Yes |
| **Anti-Debug** | — | — | Yes |
| **License System** | — | Yes | Yes |
| **Per-build Randomized Outputs** | — | — | Yes |
| **PYD Integrity Hashes** | — | — | Yes |

{: .text-center}
*All tiers include the GUI launcher. No per-build fees. No royalties.*

---

## Get Started

### 1. Download (Free)

`Labyrinx.exe` runs in **Freemium** mode out of the box — name obfuscation + native x64 .pyd output. No registration required.

{: .text-center}
[**Download Labyrinx**](https://github.com/Labyrinx-dev/Labyrinx-dev.github.io/raw/master/release/Labyrinx.exe)

{: .small}
*Includes GUI launcher + User Manual. Windows 10+, 64-bit. CLI coming next release.*

### 2. Subscribe to Upgrade

Unlock Pro or Enterprise features instantly. License key delivered automatically.

{: .text-center}
[**Subscribe — Pro ($9/mo)**](https://labyrinx.lemonsqueezy.com/checkout/buy/d9731dc3-df7c-441c-b308-310696483a92) — Levels 1–4, module encryption, string encryption  
[**Subscribe — Enterprise ($29/mo)**](https://labyrinx.lemonsqueezy.com/checkout/buy/d97c655e-ebd8-4370-b868-158498d220d3) — Level 6 VM, AES-256 module encryption, anti-debug, per-build randomized outputs

### 3. Activate

Enter your license key in Labyrinx to unlock. Valid for the duration of your subscription. Cancel anytime.

---

## System Requirements

### Build Machine (where you run Labyrinx)

| Component | Requirement |
|---|---|
| **Operating System** | Windows 10 or later, 64-bit |
| **Python** | Python 3.13 (non-Store). Recommended: `C:\Python313` |
| **Cython + setuptools** | `pip install Cython setuptools` — required for Pro and Enterprise |
| **C Compiler** | Visual Studio Build Tools 2022 with "Desktop development with C++" workload |

**Visual Studio Build Tools 2022** — [Free download](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2022). Select the **"Desktop development with C++"** workload during installation. This provides the MSVC compiler (`cl.exe`) needed to compile `.pyd` files. The Build Tools are completely free — no Visual Studio license required.

> **GCC/MinGW does NOT work** with Python 3.13 on Windows. MSVC is required.

### Target Machine (your customer)

| Component | Requirement |
|---|---|
| **Operating System** | Windows 10 or later, 64-bit |
| **Python** | **Not required** — embedded in the output folder |
| **Disk Space** | ~150 MB per application |

### Without Cython + MSVC

Labyrinx still works — obfuscated `.py` files are shipped instead of native `.pyd` machine code. This gives you Freemium-tier protection (name obfuscation). Install Cython + MSVC for full Pro/Enterprise native compilation.

---

## FAQ

**Q: Does the output need Python installed?**
A: No. The output folder contains a full embedded Python runtime (python313.dll + stdlib). Your customers just double-click.

**Q: Why folders instead of a single EXE?**
A: Folders avoid antivirus false positives (no EXE packing), start instantly (no temp extraction), and support incremental updates (swap individual .pyd files).


**Q: Can I sell software protected with Labyrinx?**
A: Yes. The built-in license key system (Enterprise) lets you generate and distribute signed license keys to your customers. Optional hardware binding, expiry dates, and tier enforcement.

**Q: What is the output size?**
A: Approximately 40 MB for a minimal app, 145 MB for a typical Flask web app, and 200-400 MB for data science applications. The Python runtime and standard library account for the base footprint.

**Q: What Windows versions are supported?**
A: Windows 10 and later, 64-bit.

**Q: Does the dependency scanner catch everything?**
A: Labyrinx performs deep import tracing to automatically discover required packages. While thorough, it may not detect 100% of dependencies — particularly packages loaded dynamically at runtime (e.g., `importlib.import_module()`), plugin-style entry points, or non-standard packaging layouts. Always test your built output thoroughly. You can add missed packages via the Hidden Imports field in the GUI.

---

{: .text-center}
## All Downloads

[Browse the release folder](https://github.com/Labyrinx-dev/Labyrinx-dev.github.io/tree/master/release) — `Labyrinx.exe` and the User Manual.  
*HWID Tool and License Creator are built from within Labyrinx (Admin panel).*

{: .text-center}
[**Browse All Downloads**](https://github.com/Labyrinx-dev/Labyrinx-dev.github.io/tree/master/release)

{: .small}
*Windows 10+, 64-bit. See [System Requirements](#system-requirements) above for full details.*<br>
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
