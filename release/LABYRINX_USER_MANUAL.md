# Labyrinx — User Manual

**Python Code Obfuscation Toolkit**  
Version 3.1 | June 2026

---

## 1. Introduction

Labyrinx is a professional Python obfuscation toolkit for Windows. It transforms Python source code into a hardened, tamper-proof form that is functionally identical but virtually impossible to reverse engineer. It includes a built-in license key system for per-customer activation and two EXE packaging options: the proprietary **LBRX Native Packager** and standard PyInstaller.

### 1.1 What Labyrinx Does

- **Obfuscates** Python source files — renames symbols, encrypts strings, flattens control flow, and more
- **Protects** the obfuscated output with anti-debug, AES-256 module encryption, and a custom 49-opcode stack VM
- **Packages** into a standalone Windows EXE via **LBRX Native** (proprietary, AES-256-CTR encrypted) or PyInstaller
- **Licenses** the EXE with HMAC-signed keys for per-customer activation
- **Delivers** as a Windows GUI application — no Python environment required

### 1.2 System Requirements

| Component | Requirement |
|---|---|
| Operating System | Windows 10 or later (64-bit) |
| Architecture | x64 |
| Disk Space | 500 MB (includes Python runtime for EXE compilation) |
| Python (for CLI) | 3.13 (not required for GUI or EXE output) |

---

## 2. Quick Start

### 2.1 Installation

1. Download **Labyrinx.exe** from your account dashboard
2. Place it anywhere on your machine (no installer required)
3. Double-click to launch the GUI

On first launch, you will be prompted to activate your license. Paste your license key and click **Activate**. Your license is saved to `%APPDATA%\Labyrinx\license.txt` — you won't be prompted again.

### 2.2 Your First Obfuscation

1. Launch Labyrinx.exe
2. Click **Browse** to select your project folder (or a single `.py` file)
3. Choose an obfuscation **Level** (start with Level 2 for testing)
4. Select your packager: **LBRX Native** (recommended) or PyInstaller
5. Click **Build**

Your protected output will be in the `dist/` folder.

### 2.3 Command-Line Quick Start

```batch
REM Obfuscate + build with LBRX Native (recommended)
"Labyrinx.exe" --input app.py --output dist/ --level 2 --native-packager

REM Obfuscate + build with PyInstaller
"Labyrinx.exe" --input app.py --output dist/ --level 2 --compile-exe

REM Full project with license
"Labyrinx.exe" --input ./src/ --output ./dist/ --level 5 --native-packager --with-license secret.bin
```

---

## 3. LBRX Native Packager

The LBRX Native Packager is Labyrinx's proprietary EXE format — designed to be resistant to standard extraction tools like `pyinstxtractor`. It uses a custom C bootloader, AES-256-CTR encryption, and per-EXE random keys.

### 3.1 Architecture

The LBRX Native Packager uses a compact C bootloader (~126 KB) that is prepended to your encrypted application payload. The bootloader handles decryption, file extraction, integrity verification, Python initialization, and cleanup — all without writing anything permanent to disk.

At a high level, the bootloader:
1. Locates and decrypts the application payload using a per-EXE key
2. Verifies the payload has not been tampered with
3. Extracts all files (Python runtime, your code, dependencies) to a temporary location
4. Verifies runtime module integrity before loading
5. Initializes Python and runs your application
6. Securely wipes all extracted files on exit

### 3.2 Security Features

| Feature | Detail |
|---------|--------|
| **Per-EXE random keys** | Every build generates a unique encryption key — no two EXEs are alike |
| **Key protection** | Encryption key is cryptographically bound to the payload — no compile-time constants to extract |
| **Integrity verification** | Full payload authenticated before any files are extracted — tampering is detected immediately |
| **Runtime hardening** | All native runtime modules verified before loading — replacement is detected and blocked |
| **Standard cryptography** | Uses Windows built-in cryptographic providers — FIPS-compliant, no hand-rolled algorithms |
| **Smart compression** | Text assets compressed automatically — saves 30-45% on payload size |
| **Zero-trace cleanup** | All extracted files securely wiped after the application exits — nothing left on disk |

### 3.3 Integrity & Tamper Detection

The bootloader cryptographically verifies the EXE before running any code. If the EXE has been modified in any way — corrupted bytes, truncated file, tampered payload — the bootloader detects it and exits immediately without executing your application.

### 3.4 Diagnostics

The bootloader writes a diagnostic log to `%TEMP%` on every run. This log is useful for troubleshooting build or startup issues — it records bootloader version, verification status, extraction progress, and any errors encountered.

### 3.5 LBRX Native vs PyInstaller

| | LBRX Native | PyInstaller |
|---|-------------|-------------|
| Format | Proprietary, encrypted | Standard (ZIP overlay) |
| Extraction resistance | pyinstxtractor-proof | Extractable with public tools |
| Per-EXE keys | Yes (random per build) | No |
| Integrity verification | SHA-256 of entire payload | None |
| .pyd hardening | Embedded hashes verified pre-import | None |
| Zero-trace cleanup | Yes (two-phase) | No (files persist in %TEMP%) |
| Compression | LZNT1 (30-45% savings) | Zlib |
| Minimum EXE size | ~29 MB | ~8 MB |
| Availability | Pro + Enterprise | All tiers |

---

## 4. Protection Levels

Labyrinx offers six escalating protection levels. Higher levels provide stronger protection but increase build time and may affect runtime performance.

**Actual tested EXE sizes and startup times (hello-world baseline, Python 3.13, Windows 10 x64):**

| Level | EXE Size | Startup | Features Active |
|-------|----------|---------|-----------------|
| 1 | 28.8 MB | 5.0s | Base obfuscation, comment/docstring removal |
| 2 | 28.8 MB | 4.9s | + Name obfuscation, import name obfuscation |
| 3 | 28.9 MB | 4.9s | + String encryption (XOR + base85) |
| 4 | 28.9 MB | 5.0s | + Full control flow flattening, dead code injection |
| 5 | 42.7 MB | 15.4s | + Anti-debug (x6), AES-256-CTR module encryption |
| 6 | 42.7 MB | 14.7s | + 49-opcode custom stack VM |

### Level 1 — Lightweight

| Feature | Included |
|---|---|
| Name obfuscation (function/variable/class names) | No (cleanup only) |
| Comment and docstring removal | Yes |
| Whitespace normalization | Yes |

**Best for:** Testing the pipeline, quick protection of non-critical code.

### Level 2 — Standard

| Feature | Included |
|---|---|
| Everything in Level 1 | Yes |
| Name obfuscation | Yes |
| Import name obfuscation | Yes |

**Best for:** General protection of internal tools and scripts.

### Level 3 — Enhanced

| Feature | Included |
|---|---|
| Everything in Level 2 | Yes |
| String obfuscation (XOR + base85 encoding) | Yes |
| Numeric literal obfuscation | Yes |

**Best for:** Commercial applications where string analysis is a concern.

### Level 4 — Advanced

| Feature | Included |
|---|---|
| Everything in Level 3 | Yes |
| Full control flow flattening | Yes |
| Dead code injection (opaque predicates) | Yes |

**Best for:** Protecting proprietary algorithms and business logic.

### Level 5 — Maximum (Module Encryption + Anti-Debug)

| Feature | Included |
|---|---|
| Everything in Level 4 | Yes |
| **AES-256-CTR module encryption** — each `.py` module encrypted, decrypted in-memory at runtime | Yes |
| Anti-debug protection (6 detection techniques) | Yes |
| Auto-includes `cryptography` for AES acceleration | Yes |
| Cache disabled (zero-trace mode) | Yes |

**Best for:** Commercial distribution, customer-delivered EXEs.

### Level 6 — Maximum + VM

| Feature | Included |
|---|---|
| Everything in Level 5 | Yes |
| **49-opcode custom stack VM** — selected functions compiled to VM bytecode | Yes |
| VM opcode scrambling (per-build randomization) | Yes |
| Opaque predicate injection in VM bytecode | Yes |

**Best for:** Maximum protection against determined reverse engineering.

---

## 5. GUI Reference

### 5.1 Main Window

The Labyrinx GUI is organized into collapsible sections:

**Project Section**
- Project Directory — browse to your Python project folder
- Main Script — optional entry point (e.g., `main.py`)
- Skip Files — comma-separated list of files to exclude from obfuscation

**Obfuscation Section**
- Level — slider from 1 to 6
- Preserve Names — comma-separated list of names to keep unchanged (e.g., public API function names)
- String Encryption — toggle (Level 3+)
- VM Obfuscation — toggle (Level 6)
- Anti-Debug — toggle (Level 5+)
- Encrypt Modules — toggle (Level 5+)

**Build Section**
- Native Packager — LBRX proprietary format (recommended)
- PyInstaller — standard packager
- Custom Icon — optional `.ico` file for the EXE
- Output Folder — where the protected output is written

**License Section** (Enterprise only)
- Enable License — embed HMAC license verification
- Secret File — path to your 16-byte license secret
- Create License for Customer — generates a signed license key

### 5.2 Build Progress

When you click **Build**, a progress dialog shows:
- Current obfuscation step
- Real-time log output
- Progress bar with phase markers (collecting dependencies, building PYZ/PKG/EXE, finalizing)
- Build complete notification with output path and EXE size

### 5.3 Progress Details (LBRX Native)

The native packager provides detailed collection stats:
- Python interpreter files collected
- Project files collected
- Site-packages collected (with count)
- .pyd runtime files collected (with integrity hash count)
- Payload size and compression savings
- Final EXE size and format version

### 5.4 Configuration Files

The launcher supports save/load of configuration presets (`.pyobfuscator_config.json`), allowing you to reuse the same settings across multiple builds.

---

## 6. License System

Labyrinx includes a built-in HMAC-SHA256 license key system for per-customer activation of compiled EXEs.

### 6.1 How It Works

1. **Generate a secret** — a 16-byte random key (keep this safe, never distribute it)
2. **Build with license** — embed verification into the EXE using the secret
3. **Create customer keys** — generate HMAC-signed keys for each customer
4. **Customer activates** — paste the key into the EXE on first launch

### 6.2 License Format

```
base64(customer_name|expiry_timestamp|tier[|hwid]).hex(HMAC-SHA256[:16])
```

Example:
```
Sm9obiBEb2V8MTc1NDA5NzIwMHxlbnRlcnByaXNl.3f7a2b1c8d9e0f4a
```

### 6.3 Tiers

| Tier | Max Level | Features |
|---|---|---|
| **Freemium** | Level 2 | Name obfuscation, PyInstaller EXE |
| **Pro** | Level 4 | + String encryption, full CF, LBRX Native, license system |
| **Enterprise** | Level 6 | + AES-256-CTR modules, anti-debug (x6), 49-opcode VM, per-EXE keys, zero-trace |

### 6.4 License Expiry

Licenses can have:
- **Fixed expiry** (e.g., 365 days from purchase)
- **Calendar-month expiry** (e.g., expires at end of billing month)
- **Lifetime** (10-year expiry)

Expired Pro/Enterprise licenses automatically downgrade to Freemium-level access rather than hard-exiting.

### 6.5 Hardware Binding (Optional)

Licenses can be bound to a specific machine using HWID (volume serial number or MAC address). The HWID is embedded in the license payload, and the EXE verifies it at runtime.

---

## 7. CLI Reference

Labyrinx.exe supports command-line operations for automation and CI/CD pipelines.

### 7.1 Build Commands

```batch
REM Obfuscate a single file (LBRX Native)
"Labyrinx.exe" --input app.py --output dist/ --native-packager

REM Obfuscate a project directory (LBRX Native)
"Labyrinx.exe" --input ./src/ --output ./dist/ --level 4 --native-packager

REM Obfuscate and compile to EXE (PyInstaller fallback)
"Labyrinx.exe" --input ./src/ --output ./dist/ --level 4 --compile-exe

REM Full pipeline with license + LBRX Native
"Labyrinx.exe" --input ./src/ --output ./build/ ^
  --level 5 --native-packager --with-license secret.bin

REM Skip specific files
"Labyrinx.exe" --input ./src/ --output ./dist/ --skip config.py,setup.py

REM With custom icon + hidden imports
"Labyrinx.exe" --input ./src/ --output ./dist/ --level 4 ^
  --native-packager --icon app.ico --hidden-import cryptography

REM Disable console window (GUI apps)
"Labyrinx.exe" --input ./src/ --output ./dist/ --level 4 ^
  --native-packager --noconsole

REM String encryption + VM (Level 6)
"Labyrinx.exe" --input app.py --output dist/ --level 6 ^
  --native-packager --string-encryption --vm-obfuscation
```

### 7.2 CLI Flags Reference

| Flag | Description |
|------|-------------|
| `--input` | Python file or project directory |
| `--output` | Output directory |
| `--level` | Obfuscation level (1-6) |
| `--native-packager` | Use LBRX Native packager (proprietary format) |
| `--compile-exe` | Use PyInstaller packager (standard format) |
| `--noconsole` | Hide console window (for GUI apps) |
| `--icon` | Path to `.ico` file |
| `--hidden-import` | Additional packages to bundle (repeatable) |
| `--string-encryption` | Enable string encryption |
| `--vm-obfuscation` | Enable VM obfuscation (Level 6) |
| `--anti-debug` | Enable anti-debug protection |
| `--encrypt-modules` | Enable AES-256-CTR module encryption |
| `--with-license` | Path to license secret file |
| `--exe-name` | Custom output EXE name |
| `--verbose` | Verbose build output |
| `--skip` | Comma-separated files to skip |

### 7.3 License Commands

```batch
REM Generate a 16-byte license secret
"Labyrinx.exe" --license-gen-secret secret.bin

REM Show the embedded key (base64 of secret)
"Labyrinx.exe" --license-show-k1 secret.bin

REM Create a customer license key
"Labyrinx.exe" --create-license secret.bin ^
  --license-customer "Acme Corp" ^
  --license-expiry-days 365 ^
  --license-tier enterprise

REM Create a license with hardware binding
"Labyrinx.exe" --create-license secret.bin ^
  --license-customer "Acme Corp" ^
  --license-expiry-days 365 ^
  --license-tier pro ^
  --license-hwid ABC123

REM Show current machine HWID
"Labyrinx.exe" --license-show-hwid
```

### 7.4 Restore Command

```batch
REM Restore an obfuscated project to its original source
"Labyrinx.exe" --restore ./backup/2026-05-01/
```

---

## 8. Anti-Debug & Tamper Protection

### 8.1 Anti-Debug Techniques (Level 5+)

Labyrinx employs six detection methods at runtime:

| Technique | Detection Target |
|---|---|
| `sys.gettrace()` | Python-level debugger (pdb, IDE debugger) |
| `IsDebuggerPresent()` | Windows user-mode debugger |
| `CheckRemoteDebuggerPresent()` | Remote debugging |
| `NtQueryInformationProcess(DebugPort)` | Kernel-mode debugger detection |
| `NtQueryInformationProcess(NtGlobalFlag)` | Heap debugging flags |
| `rdtsc` timing check | Step-through debugging (timing anomalies) |
| Hardware breakpoint scan | DR0–DR3 register check |

If any technique detects debugging, the application exits immediately with a tamper alert.

### 8.2 Payload Integrity (LBRX Native)

Every EXE built with the LBRX Native packager includes multiple layers of cryptographic verification:
- **Payload authentication** — the decrypted payload is verified before any files are extracted
- **Per-EXE random encryption** — every build uses a unique key, cryptographically bound to the payload
- **Runtime module hardening** — all native runtime files are verified before being loaded; any replacement is detected and blocked

### 8.3 String Encryption

At Level 3+, all string constants in your code are encrypted. At Level 5+, per-string encryption schemes are randomized. This prevents static analysis tools from finding embedded strings, API keys, URLs, or configuration data.

### 8.4 Ctrl+C / Shutdown Handling (LBRX Native)

The bootloader handles Ctrl+C gracefully:
- **First Ctrl+C**: runs your application's registered shutdown hook (if configured), then exits cleanly
- **Double-tap Ctrl+C**: force-exits immediately for emergency situations
- The shutdown hook is set via an environment variable — your app can register cleanup code (e.g., save database, close connections)

---

## 9. Code Virtualization

### 9.1 Overview

Level 6 compiles selected functions into a custom stack-based VM bytecode. The function body is replaced with bytecode that is:
- **Scrambled** — opcode values are randomized per build
- **Encrypted** — XOR-encrypted with a per-function key
- **Interpreted** — executed by a VM interpreter embedded in the EXE (`_lx_vm.pyd`)

### 9.2 Supported Constructs

| Python Construct | VM Support |
|---|---|
| Arithmetic/binary operations | Yes |
| Variable load/store | Yes |
| if/elif/else | Yes |
| for loops | Yes |
| while loops | Yes |
| try/except/finally | Yes |
| with statements | Yes |
| yield / generators | Yes |
| yield from | Yes |
| Decorators | Yes |
| Lambda expressions | Yes |
| Named expressions (:=) | Yes |
| Starred expressions (*args) | Yes |
| Slice operations | Yes |
| Function calls | Yes |

### 9.3 Dead Code Injection

Between every original VM instruction, junk bytecode blocks are inserted using opaque predicates (always-true/always-false conditions). Decompilation tools produce hundreds of misleading code paths.

---

## 10. Deep Import Scanning

The LBRX Native builder includes an intelligent dependency resolver:

1. **Transitive dependency resolution** — reads `Requires-Dist` from package METADATA to build the full dependency tree
2. **Deep import scan** — AST-scans `.py` files of every resolved package to find undeclared runtime imports, iterating until convergence (max 10 iterations)
3. **PyPI name mapping** — resolves package names to import names via `.dist-info/top_level.txt`, with fallback heuristics (`pycairo` → `cairo`, `python-dateutil` → `dateutil`)
4. **Sub-package matching** — automatically includes `_`-prefixed internal packages (e.g., `_plotly_utils` when `plotly` is required)

Example: specifying just `reportlab` automatically resolves:
```
reportlab → rlPyCairo → pycairo → cairo → Pillow → PIL → freetype
```

---

## 11. Stress Test Results

Labyrinx has been thoroughly tested against real open-source Python projects. **100% pass rate across 27 tests in 6 categories** (June 2026).

### Open-Source Project Tests (Level 2)

| Project | EXE Size | Build Time | Run Time |
|---------|----------|-----------|----------|
| yt-dlp 2026.3.17 | 70.2 MB | 75s | 17.9s |
| Flask 3.1.3 | 52.5 MB | 40s | 9.6s |
| Pygments 2.20.0 | 34.3 MB | 25s | 6.7s |
| SQLAlchemy 2.0.49 | 40.7 MB | 35s | 8.4s |
| Pillow 12.2.0 | 75.3 MB | 47s | 9.3s |
| pandas 3.0.3 | 190.2 MB | 151s | 26.2s |
| cryptography 48.0.0 | 42.7 MB | 28s | 6.9s |
| reportlab | 87.7 MB | 60s | 11.6s |

### Bootloader Integrity Tests

All 4 tampering attacks correctly detected:
- Magic corrupt → `FAIL:magic`, exit 1
- Payload corrupt → `FAIL:integrity`, exit 1
- Footer truncation → `FAIL:magic`, exit 1
- Key corruption → `FAIL:integrity`, exit 1

### Cleanup & Stability

- **Zero-trace**: 0 files left on disk after cache-disabled run (verified via `%LOCALAPPDATA%\Lbrx` enumeration)
- **Multi-launch**: 5 consecutive launches, all correct output, avg 2.3s startup (cached)

---

## 12. FAQ

**Q: Does obfuscation affect runtime behavior?**  
A: No. Labyrinx transforms your source code's structure, but the runtime behavior is identical. All obfuscated code passes your existing test suite unchanged.

**Q: Does Labyrinx support async/await code?**  
A: Yes. Async functions are preserved correctly across all protection levels. Note that Level 6 VM does not virtualize async functions (they pass through at Level 5 protection).

**Q: Can I use Labyrinx in an automated build pipeline?**  
A: Yes. The full CLI interface supports headless automation. All settings available in the GUI are also available via command-line flags.

**Q: What happens if a customer's license expires?**  
A: The application gracefully downgrades to Freemium-level access (Level 2 protection features). The customer can still use the software but with limited protection options.

**Q: Is the license secret safe inside the EXE?**  
A: The secret is embedded within obfuscated, AES-256 encrypted modules inside an AES-256-CTR encrypted payload with per-EXE random keys. At Level 5+, extracting the secret requires defeating multiple layers of encryption and obfuscation.

**Q: Can I rebind a license to a different machine?**  
A: Yes. Generate a new license key without HWID binding, or generate one with the new machine's HWID.

**Q: Can my LBRX Native EXE be unpacked with pyinstxtractor?**  
A: No. LBRX Native uses a proprietary format with AES-256-CTR encryption, a custom C bootloader, and no ZIP overlay. Standard PyInstaller extraction tools cannot parse LBRX EXEs.

**Q: What is the minimum EXE size?**  
A: ~29 MB for a hello-world application (includes Python 3.13 runtime, standard library, and Labyrinx `.pyd` runtime modules). The ~126 KB bootloader is negligible. Larger projects scale primarily with their dependencies.

**Q: Does LBRX Native support GUI applications (Tkinter)?**  
A: Yes. Tcl/Tk runtime (~5 MB) is automatically collected and bundled. The bootstrap script sets `TCL_LIBRARY` and `TK_LIBRARY` environment variables at startup.

---

## 13. Troubleshooting

### Build Fails at Obfuscation Step
- Ensure your project has no syntax errors (run `python -m py_compile your_file.py` first)
- Check that all imports in your code are resolvable
- Try building with Level 1 first to isolate issues

### Build Fails — Missing Packages
- Use `--hidden-import` to explicitly specify packages that auto-detection misses
- Check that the package is installed in your Python environment (`pip list`)
- For LBRX Native: the deep import scanner covers most cases, but C-extensions with unusual import patterns may need explicit `--hidden-import`

### EXE Fails to Start (LBRX Native)
- Check the bootloader diagnostic log in `%TEMP%` for error details
- Verify `dist_tools\bootloader_console.exe` exists and is the correct architecture
- Ensure Labyrinx runtime files are present in the Labyrinx directory
- If an integrity error appears, the EXE may have been corrupted — rebuild

### EXE Fails to Start (License Error)
- Verify `license.txt` is present next to the EXE
- Check the license has not expired
- If HWID-bound, run on the original machine

### Anti-Debug False Positive
- Close all debuggers, IDEs, and monitoring tools
- Check for process monitoring tools (Process Explorer, API Monitor, etc.)
- Build without anti-debug (Level 4 or below) for internal use

### Large EXE Size
- LBRX Native baseline is ~29 MB (Python 3.13 runtime + standard library + .pyd modules)
- Each large dependency adds its own footprint (pandas adds ~160 MB)
- Use `--hidden-import` sparingly — the deep import scanner is precise; only use for packages it misses
- For PyInstaller builds, use UPX compression (`--upx-dir`)

### Ctrl+C Not Working (LBRX Native)
- The bootloader's Ctrl+C handler requires a console window. GUI apps (`--noconsole`) use a different shutdown mechanism.
- Set the shutdown hook environment variable to run cleanup code on Ctrl+C (format: `module.function`)

---

## 14. Support

- **Email:** labyrinx@yahoo.com
- **Documentation:** docs.labyrinx.dev
- **License key issues:** Include your license key and HWID when contacting support
- **Bug reports:** Include the bootloader diagnostic log from `%TEMP%` and the build output

---

*Labyrinx — Python Code Obfuscation Toolkit*  
*Version 3.1 | (c) 2026 Labyrinx Software*
