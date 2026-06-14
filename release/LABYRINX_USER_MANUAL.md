# Labyrinx — User Manual

**Python Code Protection Toolkit**
Version 3.3 | June 2026

---

## 1. Introduction

Labyrinx protects Python applications by obfuscating source code, compiling it to native x64 machine code via Cython, and delivering a self-contained folder your customers can run immediately. No Python installation required on the target machine.

### 1.1 What Labyrinx Does

- **Obfuscates** Python source — renames symbols, encrypts strings, flattens control flow
- **Compiles** obfuscated code to native `.pyd` files via Cython (x64 machine code)
- **Assembles** a self-contained folder with embedded Python runtime
- **Protects** with module encryption (compression for Pro, AES-256 for Enterprise)
- **Virtualizes** selected functions into custom VM bytecode (Enterprise)
- **Licenses** applications with cryptographically signed keys (Enterprise)

### 1.2 Output Format

Labyrinx produces an **Embedded Python Folder** — not a single EXE. This approach:

- Avoids antivirus false positives (no EXE packing)
- Starts instantly (no temp extraction)
- Supports incremental updates (replace individual `.pyd` files)
- Is fully transparent and AV-friendly

### 1.3 System Requirements

**Build Machine** (where you run Labyrinx):

| Component | Requirement | Notes |
|---|---|---|
| Operating System | Windows 10 or later (64-bit) | |
| Python | **Python 3.13** (non-Store) | Recommended: `C:\Python313`. Windows Store Python works but may flash a console window during builds. |
| Cython + setuptools | `pip install Cython setuptools` | Required for Pro and Enterprise tiers only |
| C Compiler | **Visual Studio Build Tools 2022** | **MSVC only** — GCC/MinGW does NOT work with Python 3.13 on Windows. [Free download](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2022) — select "Desktop development with C++" workload. |

**Target Machine** (your customer):

| Component | Requirement |
|---|---|
| Operating System | Windows 10 or later (64-bit) |
| Python | **Not required** — embedded in the output |
| Disk Space | ~150 MB per build output |

**Tier-specific requirements:**

| Tier | Python | Cython + setuptools | VS Build Tools |
|---|---|---|---|
| **Freemium** | Not required | Not required | Not required |
| **Pro** | Required | Required | Required |
| **Enterprise** | Required | Required | Required |

> **Without Cython + MSVC:** The build still succeeds, but `.py` files are shipped as obfuscated source instead of native `.pyd` machine code — Freemium-tier protection. Install the dependencies above for full Pro/Enterprise protection.

---

## 2. Quick Start

### 2.1 Installation

1. Download **Labyrinx.zip** from your account dashboard
2. Extract to any location (no installer required)
3. Double-click `Labyrinx.exe` to launch the GUI

On first launch, you may be prompted to accept the EULA and activate your license. Paste your license key and click **Activate**.

### 2.2 Your First Build

1. Launch **Labyrinx.exe**
2. Click **Browse** to select your project folder
3. Choose a preset (e.g., "Flask Web App" for web apps, "CLI Tool" for command-line tools)
4. Click **Build**

Your protected output will be in `dist/YourApp/` — zip it and ship it.

### 2.3 Next Release: Command-Line Interface

A CLI tool (`labyrinx-cli.exe`) for CI/CD automation is planned for the next release. For now, all builds use the GUI launcher.

---

## 3. Protection Levels

Labyrinx offers six cumulative protection levels.

### Level 1 — Lightweight
- Comment and docstring removal
- **Best for:** Testing the pipeline

### Level 2 — Standard
- Everything in Level 1
- Name obfuscation (functions, variables, classes → random tokens)
- **Best for:** Internal tools, open-source projects

### Level 3 — Enhanced
- Everything in Level 2
- String encoding (base64, AES, or random schemes)
- Dead code injection (opaque predicates)
- **Best for:** Commercial applications

### Level 4 — Advanced
- Everything in Level 3
- Control flow flattening (switch-state machines)
- **Best for:** Protecting proprietary algorithms

### Level 5 — Maximum
- Everything in Level 4
- Module encryption (compression or AES-256)
- Anti-debug protection (6 detection techniques)
- **Best for:** Customer-delivered applications

### Level 6 — Maximum + VM
- Everything in Level 5
- Code Virtualization — custom stack VM (Enterprise)
- Per-build randomized VM opcodes
- Opaque predicate injection in bytecode
- **Best for:** Maximum protection against determined reverse engineering

### Module Encryption Comparison

| | Pro | Enterprise |
|---|---|---|
| **Mechanism** | Compression | AES-256 encryption |
| **After .pyd decompilation** | Compressed data visible | Encrypted ciphertext |
| **Build time** | ~75 seconds | ~90 seconds |

Both schemes protect your source by wrapping it before compilation, keeping build times fast even for large projects.

---

## 4. GUI Reference

### 4.1 Main Window

The Labyrinx GUI is organized into sections:

**Project Section**
- Project Directory — browse to your Python project folder
- Main Script — entry point (e.g., `waitress_server.py`)
- Skip Files — comma-separated files to exclude from obfuscation

**Obfuscation Section**
- Level — dropdown from 1 to 6 (clamped to tier max)
- Presets — one-click configuration (Flask Web App, CLI Tool, Minimal)
- Name Obfuscation — toggle
- String Encryption — toggle with scheme selector (base64, AES, random)
- Anti-Debug — toggle (Enterprise only)
- Module Encryption — toggle (Pro: gzip, Enterprise: AES)
- VM Obfuscation — toggle (Enterprise only)

**Build Section**
- Custom Icon — optional `.ico` file
- Output Folder — where `dist/` is written

**License Section** (Enterprise only)
- Enable License — embed verification in the build
- Secret File — path to your license secret
- Create License — generate signed customer keys

### 4.2 Configuration Presets

| Preset | Level | String Encryption | Module Encryption | Best For |
|---|---|---|---|---|
| Flask Web App | 4 | Off | Yes (gzip/AES) | Web apps |
| Minimal | 1 | Off | No | Quick tests |

### 4.3 Build Progress

The build progress dialog shows:
- Real-time log output with file-by-file progress
- Phase markers (obfuscating, compiling, collecting, writing)
- Final output path and size
- Build time

---

## 5. CLI Reference

A command-line interface (`labyrinx-cli.exe`) for CI/CD automation and headless builds is planned for the **next release**. It will support:

- Project and single-file obfuscation
- All protection levels and features (string encryption, module encryption, VM, anti-debug)
- License key generation and management
- JSON output for pipeline integration

For now, all builds use the GUI launcher. If you need CLI access for a specific workflow, contact support@labyrinx.dev.

---

## 6. License System

Labyrinx includes a built-in license key system (Enterprise tier) for per-customer activation.

### 6.1 How It Works

1. **Generate a secret** — a random encryption key (keep this safe, never distribute)
2. **Build with license** — embed verification into the app using the secret
3. **Create customer keys** — generate signed keys for each customer
4. **Customer activates** — places `license.txt` next to the app, launches, and activates

### 6.2 License Format

License keys are cryptographically signed tokens containing the customer identity, expiry, tier, and optional hardware binding. Keys are verified at runtime using Windows native cryptography — no Python-level crypto is exposed to debuggers.

### 6.3 Tiers

| Tier | Max Level | Key Features |
|---|---|---|
| Freemium | 2 | Name obfuscation, Cython .pyd |
| Pro | 4 | + String encryption, CF flattening, gzip module encryption |
| Enterprise | 6 | + AES module encryption, VM, anti-debug, hardware locking |

### 6.4 License Expiry

- **Fixed expiry** — e.g., 365 days from purchase
- **Calendar-month expiry** — expires at end of billing period
- **Lifetime** — extended expiry

Expired licenses gracefully downgrade to Freemium rather than hard-exiting.

### 6.5 Hardware Binding (Optional)

Bind licenses to specific machines using hardware identifiers. Customers obtain their HWID by running the included `get_hwid.exe` tool.

---

## 7. Output & Distribution

### 7.1 Output Structure

```
dist/MyApp/
├── MyApp.exe              ← click-to-run launcher
├── MyApp.bat              ← fallback batch launcher
├── python313.dll          ← embedded Python interpreter
├── python313.zip          ← compressed standard library
├── python313._pth         ← path configuration
├── vcruntime140*.dll      ← Visual C++ runtime
├── *.pyd                  ← your application code (native x64)
├── Labyrinx runtime        ← protection modules (encryption, VM, integrity)
└── Lib/site-packages/     ← third-party dependencies
```

### 7.2 Distributing to Customers

1. **Zip** the entire `dist/MyApp/` folder
2. **Ship** the zip file
3. Customer **extracts** and double-clicks `MyApp.exe` (or `MyApp.bat`)
4. On first launch, customer activates their license (if Enterprise)

No Python installation. No command lines. No dependencies.

### 7.3 Incremental Updates

Replace individual `.pyd` files to update your app. No need to resend the full 145 MB — just send the changed modules.

### 7.4 Typical Output Sizes

| Project | Output Size | Files |
|---|---|---|
| Minimal CLI app | ~40 MB | ~2,000 |
| Flask web app | ~145 MB | ~5,200 |
| Data science app | ~200-400 MB | ~8,000+ |

---

## 8. Anti-Debug & Tamper Protection

### 8.1 Anti-Debug Protection (Enterprise)

Multiple detection techniques guard against debugging and reverse engineering at runtime:

- Python-level debugger detection
- User-mode and kernel-mode debugger detection
- Debug heap and breakpoint detection
- Timing-based single-step analysis detection

Detection triggers immediate application termination. All checks operate at the native code level and produce no exception handlers or diagnostic output.

### 8.2 File Integrity Verification (Enterprise)

Every compiled file in the output is cryptographically hashed at build time. At startup, all hashes are verified before any application code loads. Tampered or replaced files are detected and blocked before execution begins.

---

## 9. Code Virtualization (Level 6 — Enterprise)

Selected functions are compiled to custom bytecode for a private stack-based VM.

### 9.1 VM Architecture

- **Stack-based execution** — operations push and pop from a virtual stack
- **Per-build randomized opcodes** — no two builds use the same mapping
- **Encrypted bytecode** — stored in an opaque format on disk
- **Dead code injection** — junk instructions via opaque predicates
- **Covers standard Python** — control flow, exceptions, functions, generators, context managers, and comprehensions

> The VM architecture and instruction set are proprietary and intentionally undocumented.

---

## 10. Real-World Performance

Tested on a real Flask application with 20 modules, 24 compiled files, ~40 Python packages:

| Tier | Level | Features | Build Time | Output Size |
|---|---|---|---|---|
| Freemium | 2 | Name obfuscation | 40 seconds | ~151 MB |
| Pro | 4 | + gzip encrypt + CF | 75 seconds | ~140 MB |
| Enterprise | 6 | + AES encrypt + VM + anti-debug | 90 seconds | ~153 MB |

Environment: Windows 10 x64, Python 3.13, Cython + MSVC. All builds produce fully functional applications.

---

## 11. Troubleshooting

### Build Fails at Obfuscation Step
- Ensure your project has no syntax errors
- Run `python -m py_compile your_file.py` first
- Try Level 1 to isolate issues

### Cython Not Found / No .pyd Compilation

The build log shows: `WARNING: 0 modules compiled to .pyd — no native protection!`

**Install the required tools:**

1. **Install Python 3.13** (if not already installed)
   - Download from [python.org](https://www.python.org/downloads/)
   - Install to `C:\Python313` (recommended)
   - **Avoid Windows Store Python** — it may flash console windows during builds

2. **Install Cython + setuptools:**
   ```
   pip install Cython setuptools
   ```

3. **Install Visual Studio Build Tools 2022:**
   - Download: https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2022
   - Run the installer, select the **"Desktop development with C++"** workload
   - This installs MSVC v143 compiler + Windows SDK (~3 GB)
   - The Build Tools are **free** — no Visual Studio license required

4. **Verify everything works:**
   ```
   python -c "import Cython, setuptools; print('OK')"
   ```

> **GCC/MinGW does NOT work** — Python 3.13 on Windows requires MSVC for `.pyd` compilation.

### EXE Fails to Start
- Try the `.bat` file as fallback (more diagnostic output)
- Check that the output folder is complete (all files present)
- If running from a network drive, copy to local disk first

### License Verification Fails
- Verify the license file is present next to the EXE
- Check the license has not expired
- If HWID-bound, run on the original machine
- Verify the secret used to build matches the secret used to create the key

### Large Output Size
- Baseline is ~40-150 MB (Python runtime + stdlib + dependencies)
- Each large dependency adds its own footprint
- Use `--hidden-import` sparingly — the import scanner is precise

---

## 12. Support

- **Email:** labyrinx@yahoo.com
- **Website:** [labyrinx-dev.github.io](https://labyrinx-dev.github.io/)
- **Bug reports:** Include the build output log

---

*Labyrinx — Python Code Protection*
*Version 3.3 | (c) 2026 Labyrinx Software*
