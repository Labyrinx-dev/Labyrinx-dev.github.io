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
- **Protects** with module encryption (gzip for Pro, AES-256-CTR for Enterprise)
- **Virtualizes** selected functions into custom VM bytecode (Enterprise)
- **Licenses** applications with cryptographically signed keys (Enterprise)

### 1.2 Output Format

Labyrinx produces an **Embedded Python Folder** — not a single EXE. This approach:

- Avoids antivirus false positives (no EXE packing)
- Starts instantly (no temp extraction)
- Supports incremental updates (replace individual `.pyd` files)
- Is fully transparent and AV-friendly

### 1.3 System Requirements

| Component | Requirement |
|---|---|
| Operating System | Windows 10 or later (64-bit) |
| Cython | `pip install Cython setuptools` |
| C Compiler | Visual Studio Build Tools or GCC |
| Target Machine | Windows 10+ (no Python required) |
| Disk Space | ~150 MB per build output |

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
- Module encryption (gzip or AES-256-CTR)
- Anti-debug protection (6 detection techniques)
- **Best for:** Customer-delivered applications

### Level 6 — Maximum + VM
- Everything in Level 5
- Code Virtualization (49-opcode stack VM)
- Per-build randomized VM opcodes
- Opaque predicate injection in bytecode
- **Best for:** Maximum protection against determined reverse engineering

### Module Encryption Comparison

| | Pro (gzip) | Enterprise (AES) |
|---|---|---|
| **Mechanism** | Gzip compression + base64 | AES-256-CTR encryption |
| **Cython speed** | Instant (2-line stub) | Instant (2-line stub) |
| **Runtime PYDs** | 1 | 5 |
| **Decompilation** | Gzip stream visible | Opaque ciphertext |
| **Build time** | ~75 seconds | ~90 seconds |

Both schemes wrap the obfuscated source into a blob so Cython only compiles a trivial decrypt stub — making builds 4-6× faster than compiling the full obfuscated Python directly.

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
- Secret File — path to your 16-byte license secret
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

1. **Generate a secret** — 16-byte random key (keep this safe, never distribute)
2. **Build with license** — embed verification into the app using the secret
3. **Create customer keys** — generate signed keys for each customer
4. **Customer activates** — places `license.txt` next to the app, launches, and activates

### 6.2 License Format

```
base64(customer_name|expiry_timestamp|tier[|HWID]).hex-lowercase(HMAC-SHA256[:16])
```

Cryptographically signed via HMAC-SHA256 using Windows BCrypt API.

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

Bind licenses to specific machines using disk volume serial numbers. Customers obtain their HWID by running the included `get_hwid.exe` tool.

---

## 7. Output & Distribution

### 7.1 Output Structure

```
dist/MyApp/
├── MyApp.exe              ← click-to-run (~30 KB C launcher)
├── MyApp.bat              ← fallback batch launcher
├── python313.dll          ← embedded Python interpreter
├── python313.zip          ← compressed standard library
├── python313._pth         ← path configuration
├── vcruntime140*.dll      ← Visual C++ runtime
├── .__launcher__.py       ← entry point script
├── *.pyd                  ← your application code (native x64)
├── _lx_*.pyd              ← Labyrinx runtime (encryption, VM, integrity)
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

### 8.1 Anti-Debug Techniques (Enterprise)

Six detection methods run at module import time:

| # | Technique | Method |
|---|---|---|
| 1 | Python debugger | `sys.gettrace()` |
| 2 | User-mode debugger | `kernel32.IsDebuggerPresent()` |
| 3 | Kernel debugger | `NtQueryInformationProcess` → DebugPort |
| 4 | Debug heap | PEB.NtGlobalFlag via `ReadProcessMemory` |
| 5 | Single-stepping | `QueryPerformanceCounter` timing check |
| 6 | Hardware breakpoints | `GetThreadContext` → DR0-DR3 |

Detection triggers `os._exit(0)` — hard exit with no exception handlers. All checks are wrapped in `try/except` and silently pass on non-Windows platforms.

### 8.2 PYD Integrity Verification (Enterprise)

Every `.pyd` file in the output is hashed. A `.__pyd_hashes__.json` manifest is generated at build time. At runtime, the integrity module verifies hashes before loading — tampered or replaced `.pyd` files are detected and blocked.

---

## 9. Code Virtualization (Level 6 — Enterprise)

Selected functions are compiled to custom bytecode for a 49-opcode stack VM.

### 9.1 VM Architecture

- **Stack-based** — all operations push/pop from a Python list stack
- **Register-indirect locals** — fast variable access via indexed array
- **Per-build randomized opcodes** — no two builds use the same mapping
- **XOR-encrypted bytecode** — stored as base85 blobs
- **Dead code injection** — junk bytecode via opaque predicates

### 9.2 Supported Constructs

| Category | Supported |
|---|---|
| Control flow | if/elif/else, while, for, break, continue |
| Exceptions | try/except/finally, raise |
| Functions | def, return, decorators, lambdas |
| Generators | yield, yield from, generator expressions |
| Context managers | with statements |
| Expressions | walrus, starred, slice, chained comparisons |
| Comprehensions | List, Set, Dict |

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

### Cython Not Found
- Install: `pip install Cython setuptools`
- Install Visual Studio Build Tools (MSVC) or GCC
- Ensure Python is on PATH or install at `C:\Python313`

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
