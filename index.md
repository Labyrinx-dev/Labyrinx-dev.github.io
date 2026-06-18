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

<table style="margin: 0 auto;">
  <thead><tr><th></th><th>Freemium</th><th>Pro</th><th>Enterprise</th></tr></thead>
  <tbody>
    <tr><td><strong>Price</strong></td><td>Free</td><td>$9/month</td><td>$29/month</td></tr>
    <tr><td><strong>Max Level</strong></td><td>2</td><td>4</td><td>7</td></tr>
    <tr><td><strong>String Encryption (AES)</strong></td><td>—</td><td>✅</td><td>✅</td></tr>
    <tr><td><strong>Module Encryption</strong></td><td>—</td><td>✅</td><td>—</td></tr>
    <tr><td><strong>Module Encryption (AES-256)</strong></td><td>—</td><td>—</td><td>✅</td></tr>
    <tr><td><strong>Code VM</strong></td><td>—</td><td>—</td><td>✅</td></tr>
    <tr><td><strong>Anti-Debug</strong></td><td>—</td><td>—</td><td>✅</td></tr>
    <tr><td><strong>License System</strong></td><td>—</td><td>✅</td><td>✅</td></tr>
    <tr><td><strong>Per-build Randomized Outputs</strong></td><td>—</td><td>—</td><td>✅</td></tr>
    <tr><td><strong>PYD Integrity Hashes</strong></td><td>—</td><td>—</td><td>✅</td></tr>
  </tbody>
</table>

<p align="center"><em>All tiers include the GUI launcher. No per-build fees. No royalties.</em></p>

---

## Get Started

### 1. Download (Free)

Labyrinx runs in **Freemium mode** at no cost — protection levels 1–2, no license required.

<p align="center">
  <a href="https://github.com/Labyrinx-dev/Labyrinx-dev.github.io/releases/download/v1.0/Labyrinx_setup.zip"
     onclick="gtag('event', 'download', { event_category: 'engagement', event_label: 'Labyrinx Setup Zip', value: 1 });"
     style="display:inline-block;background:#1e3a5f;color:white;padding:14px 32px;border-radius:6px;text-decoration:none;font-size:16px;font-weight:bold;">
    📥 Download Labyrinx (ZIP)
  </a>
</p>

<p align="center"><em>~34 MB ZIP &middot; Extract and run Labyrinx_setup.exe &middot; Windows 10+ 64-bit &middot; No Python install needed</em></p>

<p align="center" style="background:#fff3cd;border:1px solid #ffc107;border-radius:6px;padding:12px 16px;max-width:560px;margin:12px auto;font-size:14px;">
📦 <strong>After downloading, extract the ZIP</strong>, then run <code>Labyrinx_setup.exe</code> inside.<br>
⚠️ Windows may show a SmartScreen warning — click <strong>"More info"</strong> → <strong>"Run anyway"</strong>.<br>
<em>This is normal for new software. The warning goes away after enough users trust the app.</em>
</p>

### 2. Upgrade to Pro or Enterprise

Unlock higher protection levels with a monthly subscription. License key delivered automatically after purchase.

<p align="center">
  <a href="https://labyrinx.lemonsqueezy.com/checkout/buy/d9731dc3-df7c-441c-b308-310696483a92"
     style="display:inline-block;background:#3b82f6;color:white;padding:14px 32px;border-radius:6px;text-decoration:none;font-size:16px;font-weight:bold;margin:0 8px;">
    Subscribe — Pro &middot; $9/mo
  </a>
  <a href="https://labyrinx.lemonsqueezy.com/checkout/buy/d97c655e-ebd8-4370-b868-158498d220d3"
     style="display:inline-block;background:#7c3aed;color:white;padding:14px 32px;border-radius:6px;text-decoration:none;font-size:16px;font-weight:bold;margin:0 8px;">
    Subscribe — Enterprise &middot; $29/mo
  </a>
</p>

<table style="margin: 0 auto;">
  <thead><tr><th></th><th>Pro</th><th>Enterprise</th></tr></thead>
  <tbody>
    <tr><td><strong>Price</strong></td><td>$9/month</td><td>$29/month</td></tr>
    <tr><td><strong>Protection</strong></td><td>Levels 1–4</td><td>Levels 1–7</td></tr>
    <tr><td><strong>String Encryption</strong></td><td>✅</td><td>✅</td></tr>
    <tr><td><strong>Module Encryption</strong></td><td>✅</td><td>✅ (AES-256)</td></tr>
    <tr><td><strong>Code VM</strong></td><td>—</td><td>✅</td></tr>
    <tr><td><strong>Anti-Debug</strong></td><td>—</td><td>✅</td></tr>
    <tr><td><strong>License System</strong></td><td>✅</td><td>✅</td></tr>
  </tbody>
</table>

<p align="center"><em>Subscription auto-renews monthly. Cancel anytime. License key emailed automatically after purchase.</em></p>

### 3. Activate

Open Labyrinx.exe, click the license button in the bottom bar, paste your license key. Pro/Enterprise features unlock instantly.

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


**Q: Builds are slow — any way to speed them up?**
A: Windows Defender real-time scanning inspects every `.c` and `.pyd` file as Cython writes them, which can double compile time. **Temporarily exclude your project folder** in Windows Security → Virus & threat protection → Manage settings → Exclusions → Add your project directory. Just remember to remove the exclusion after the build.

**Q: Can I sell software protected with Labyrinx?**
A: Yes. The built-in license key system (Enterprise) lets you generate and distribute signed license keys to your customers. Optional hardware binding, expiry dates, and tier enforcement.

**Q: What is the output size?**
A: Approximately 40 MB for a minimal app, 145 MB for a typical Flask web app, and 200-400 MB for data science applications. The Python runtime and standard library account for the base footprint.

**Q: What Windows versions are supported?**
A: Windows 10 and later, 64-bit.

**Q: Does the dependency scanner catch everything?**
A: Labyrinx performs deep import tracing to automatically discover required packages. While thorough, it may not detect 100% of dependencies — particularly packages loaded dynamically at runtime (e.g., `importlib.import_module()`), plugin-style entry points, or non-standard packaging layouts. Always test your built output thoroughly. You can add missed packages via the Hidden Imports field in the GUI.

---

## Learn More

{: .text-center}
📖 **[Python Code Protection Approaches Compared](/compare/)** — Folder-based embedded distribution vs single-EXE packing vs bytecode obfuscation vs transpiler compilation. Honest side-by-side comparison.

{: .text-center}
🛡️ **[How to Protect Python Source Code](/protect-python-code/)** — Complete guide: name obfuscation, native code compilation, AES encryption, and code virtualization. What each layer can and cannot do.

{: .text-center}
🔬 **[Python Obfuscation Techniques](/python-obfuscation/)** — Deep dive into all 7 obfuscation layers with code examples. From basic name mangling to custom VM bytecode.

{: .text-center}
📦 **[How to Compile Python to a Standalone Windows App](/compile-python-to-exe/)** — EXE bundling vs transpiler compilation vs native library compilation vs Labyrinx compared. Why folder-based distribution avoids antivirus false positives.

{: .text-center}
📝 **[Labyrinx Blog](/blog/)** — Articles about Python code protection, obfuscation techniques, and shipping Python desktop applications.

---

{: .text-center}
Labyrinx is available now. [Download for free](#get-started) or [subscribe](#get-started) to unlock Enterprise protection.

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

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "SoftwareApplication",
  "name": "Labyrinx",
  "applicationCategory": "DeveloperApplication",
  "operatingSystem": "Windows 10, Windows 11",
  "description": "Python code obfuscation and protection tool. Compile to native x64 machine code via Cython with 7 layers of protection including name obfuscation, string encryption, control flow flattening, module encryption, anti-debug, and code virtualization.",
  "url": "https://labyrinx-dev.github.io/",
  "offers": [
    {
      "@type": "Offer",
      "name": "Freemium",
      "price": "0",
      "priceCurrency": "USD"
    },
    {
      "@type": "Offer",
      "name": "Pro",
      "price": "9.00",
      "priceCurrency": "USD",
      "description": "Monthly subscription — protection levels 1-4, string encryption, module encryption, license system"
    },
    {
      "@type": "Offer",
      "name": "Enterprise",
      "price": "29.00",
      "priceCurrency": "USD",
      "description": "Monthly subscription — protection levels 1-7, AES-256 module encryption, code VM, anti-debug, PYD integrity hashes"
    }
  ],
  "author": {
    "@type": "Organization",
    "name": "Labyrinx",
    "email": "labyrinx@yahoo.com",
    "url": "https://github.com/Labyrinx-dev"
  },
  "browserRequirements": "Windows 10 or later, 64-bit",
  "downloadUrl": "https://github.com/Labyrinx-dev/Labyrinx-dev.github.io/releases/download/v1.0/Labyrinx_setup.exe",
  "fileSize": "33 MB",
  "softwareVersion": "1.0",
  "datePublished": "2026-06-16"
}
</script>
