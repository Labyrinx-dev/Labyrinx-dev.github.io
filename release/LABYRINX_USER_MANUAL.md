# Labyrinx — User Manual

**Python Code Obfuscation Toolkit**  
Version 3.2 | June 2026

---

## 1. Introduction

Labyrinx is a professional Python obfuscation toolkit for Windows. It transforms Python source code into a hardened form that is functionally identical but highly resistant to reverse engineering. It includes a built-in license key system for per-customer activation and two EXE packaging options: the proprietary **LBRX Native Packager** and the standard packager.

### 1.1 What Labyrinx Does

- **Obfuscates** Python source files — renames symbols, encrypts strings, flattens control flow, and more
- **Protects** the obfuscated output with anti-debug, module encryption, and a custom stack-based code VM
- **Packages** into a standalone Windows EXE via **LBRX Native** (proprietary, encrypted) or standard packager
- **Licenses** the EXE with signed keys for per-customer activation
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

1. Download **Labyrinx.exe** from the download link provided with your license
2. Place it anywhere on your machine (no installer required)
3. Double-click to launch the GUI

On first launch, you will be prompted to activate your license. Paste your license key and click **Activate**. Your license is saved and you won't be prompted again.

### 2.2 Your First Obfuscation

1. Launch Labyrinx.exe
2. Click **Browse** to select your project folder (or a single `.py` file)
3. Choose an obfuscation **Level** (start with Level 2 for testing)
4. Select your packager: **LBRX Native** (recommended) or standard
5. Click **Build**

Your protected output will be in the `dist/` folder.

### 2.3 Command-Line Quick Start

The CLI tool (`labyrinx-cli.exe`) supports full automation:

```batch
REM Obfuscate a project with LBRX Native
labyrinx-cli.exe -p ./my_project --level 4 --native-packager --exe-name MyApp

REM Obfuscate + build with standard packager
labyrinx-cli.exe -p ./my_project --level 4 --compile-exe --exe-name MyApp

REM Full pipeline with license
labyrinx-cli.exe -p ./my_project --level 5 --native-packager --with-license secret.key
```

---

## 3. LBRX Native Packager

The LBRX Native Packager is Labyrinx's proprietary EXE format — designed to be resistant to common extraction tools. It uses a custom native bootloader, per-build encryption, and a fully encrypted container format.

### 3.1 Architecture

The LBRX Native Packager uses a compact bootloader that is prepended to your encrypted application payload. The bootloader handles decryption, file extraction, integrity verification, Python initialization, and cleanup — all without writing anything permanent to disk.

At a high level, the bootloader:
1. Locates and decrypts the application payload using a per-build key
2. Verifies the payload has not been tampered with
3. Extracts all files (Python runtime, your code, dependencies) to a temporary location
4. Verifies runtime module integrity before loading
5. Initializes Python and runs your application
6. Securely wipes all extracted files on exit

### 3.2 Security Features

| Feature | Detail |
|---------|--------|
| **Per-build random keys** | Every build generates a unique encryption key — no two EXEs are alike |
| **Key protection** | Encryption key is cryptographically bound to the payload — no constants to extract |
| **Integrity verification** | Full payload authenticated before any files are extracted |
| **Runtime hardening** | Runtime modules verified before loading — replacement is detected and blocked |
| **Standard cryptography** | Uses industry-standard algorithms via the operating system's cryptographic providers |
| **Smart compression** | Text assets compressed automatically — significant payload savings |
| **Zero-trace cleanup** | All extracted files securely wiped after the application exits |

### 3.3 Integrity & Tamper Detection

The bootloader cryptographically verifies the EXE before running any code. If the EXE has been modified — corrupted bytes, truncated file, tampered payload — the bootloader detects it and exits immediately without executing your application.

### 3.4 Diagnostics

The bootloader writes a diagnostic log to the system temporary directory on every run. This log is useful for troubleshooting build or startup issues — it records version information, verification status, extraction progress, and any errors encountered.

### 3.5 LBRX Native vs Standard Packager

| | LBRX Native | Standard Packager |
|---|-------------|-------------------|
| Format | Proprietary, encrypted | Standard |
| Extraction resistance | Resistant to common tools | Extractable with public tools |
| Per-build keys | Yes | No |
| Integrity verification | Full payload | None |
| Runtime hardening | Verified pre-load | None |
| Zero-trace cleanup | Yes | No |
| Compression | Native | Standard |
| Minimum EXE size | ~29 MB | ~8 MB |
| Availability | Pro + Enterprise | All tiers |

---

## 4. Protection Levels

Labyrinx offers six escalating protection levels. Higher levels provide stronger protection but increase build time.

**Typical EXE sizes and startup times (baseline application, Python 3.13, Windows 10 x64):**

| Level | EXE Size | Startup | Features Active |
|-------|----------|---------|-----------------|
| 1 | ~29 MB | Instant | Base obfuscation, comment/docstring removal |
| 2 | ~29 MB | Instant | + Name obfuscation, import name obfuscation |
| 3 | ~29 MB | Instant | + String encryption |
| 4 | ~29 MB | Instant | + Control flow flattening, dead code injection |
| 5 | ~43 MB | Fast | + Anti-debug, module encryption |
| 6 | ~43 MB | Fast | + Code virtualization |

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
| String obfuscation | Yes |
| Numeric literal obfuscation | Yes |

**Best for:** Commercial applications where string analysis is a concern.

### Level 4 — Advanced

| Feature | Included |
|---|---|
| Everything in Level 3 | Yes |
| Control flow flattening | Yes |
| Dead code injection | Yes |

**Best for:** Protecting proprietary algorithms and business logic.

### Level 5 — Maximum

| Feature | Included |
|---|---|
| Everything in Level 4 | Yes |
| Module encryption — each `.py` module encrypted, decrypted in-memory at runtime | Yes |
| Anti-debug protection (multiple detection techniques) | Yes |
| Zero-trace mode | Yes |

**Best for:** Commercial distribution, customer-delivered EXEs.

### Level 6 — Maximum + VM

| Feature | Included |
|---|---|
| Everything in Level 5 | Yes |
| Custom stack-based code VM — selected functions compiled to VM bytecode | Yes |
| VM opcode randomization (per-build) | Yes |
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
- Standard Packager — standard format
- Custom Icon — optional `.ico` file for the EXE
- Output Folder — where the protected output is written

**License Section** (Enterprise only)
- Enable License — embed license verification
- Secret File — path to your license secret
- Create License for Customer — generates a signed license key

### 5.2 Build Progress

When you click **Build**, a progress dialog shows:
- Current obfuscation step
- Real-time log output
- Progress bar with phase markers (collecting dependencies, building, finalizing)
- Build complete notification with output path and EXE size

### 5.3 Progress Details (LBRX Native)

The native packager provides detailed collection stats:
- Python interpreter files collected
- Project files collected
- Dependencies collected (with count)
- Runtime files collected (with integrity hash count)
- Payload size and compression savings
- Final EXE size and format version

### 5.4 Configuration Files

The launcher supports save/load of configuration presets, allowing you to reuse the same settings across multiple builds.

---

## 6. License System

Labyrinx includes a built-in license key system for per-customer activation of compiled EXEs.

### 6.1 How It Works

1. **Generate a secret** — a random key file (keep this safe, never distribute it)
2. **Build with license** — embed verification into the EXE using the secret
3. **Create customer keys** — generate signed keys for each customer
4. **Customer activates** — enters the key into the EXE on first launch

### 6.2 License Format

License keys are text strings that encode customer name, expiry timestamp, tier, and optional hardware binding, cryptographically signed to prevent forgery.

### 6.3 Tiers

| Tier | Max Level | Features |
|---|---|---|
| **Freemium** | Level 2 | Name obfuscation, standard packager |
| **Pro** | Level 4 | + String encryption, full CF, LBRX Native, license system |
| **Enterprise** | Level 6 | + Module encryption, anti-debug, code VM, per-build keys, zero-trace |

### 6.4 License Expiry

Licenses can have:
- **Fixed expiry** (e.g., 365 days from purchase)
- **Calendar-month expiry** (e.g., expires at end of billing month)
- **Lifetime** (extended expiry)

Expired Pro/Enterprise licenses gracefully downgrade to Freemium-level access rather than hard-exiting.

### 6.5 Hardware Binding (Optional)

Licenses can be bound to a specific machine using a hardware identifier. The HWID is embedded in the license payload, and the EXE verifies it at runtime. Customers obtain their HWID by running the included `get_hwid.exe` tool.

---

## 7. CLI Reference

The CLI tool (`labyrinx-cli.exe`) supports command-line operations for automation and CI/CD pipelines. Use `labyrinx-cli.exe --help` for the complete flag reference.

### 7.1 Build Commands

```batch
REM Obfuscate a project (LBRX Native, recommended)
labyrinx-cli.exe -p ./src -l 4 --native-packager --exe-name MyApp

REM Obfuscate a single file
labyrinx-cli.exe script.py -o script_obf.py -l 3

REM Full pipeline with maximum protection
labyrinx-cli.exe -p ./src waitress_server.py -l 6 ^
  --string-encryption --string-scheme random ^
  --anti-debug --encrypt-modules --vm-obfuscation ^
  --native-packager --exe-name MyApp

REM With license + custom icon
labyrinx-cli.exe -p ./src -l 5 --native-packager ^
  --with-license secret.key --icon app.ico --exe-name MyApp

REM Skip specific files
labyrinx-cli.exe -p ./src --skip-files config.py,setup.py ^
  --native-packager --exe-name MyApp

REM GUI application (no console window)
labyrinx-cli.exe -p ./src -l 4 --native-packager ^
  --noconsole --exe-name MyApp

REM CI/CD pipeline mode (JSON output)
labyrinx-cli.exe -p ./src -l 6 --native-packager ^
  --exe-name MyApp --json
```

### 7.2 Key CLI Flags

| Flag | Description |
|------|-------------|
| `-p`, `--project` | Project directory |
| `-l`, `--level` | Obfuscation level (1-6) |
| `-o`, `--output` | Output file |
| `--native-packager` | Use LBRX Native packager |
| `--compile-exe` | Use standard packager |
| `--exe-name` | Custom output EXE name |
| `--noconsole` | Hide console window |
| `--icon` | Path to `.ico` file |
| `--hidden-import` | Additional packages (repeatable) |
| `--add-data` | Additional data files (format: source;dest) |
| `--string-encryption` | Enable string encryption |
| `--string-scheme` | Scheme: base64, rot13, xor, random, aes |
| `--vm-obfuscation` | Enable code VM (Level 6) |
| `--anti-debug` | Enable anti-debug |
| `--encrypt-modules` | Enable module encryption |
| `--with-license` | Embed license verification (path to secret) |
| `--verbose` | Verbose output |
| `--json` | JSON output (for CI/CD) |
| `--dry-run` | Analyze without writing |
| `--verify` | Verify obfuscated output |
| `--restore` | Restore project from backup |

### 7.3 License Commands

```batch
REM Generate a license secret
labyrinx-cli.exe --license-gen-secret secret.key

REM Create a customer license key
labyrinx-cli.exe --create-license secret.key ^
  --license-customer "Acme Corp" ^
  --license-expiry-days 365 ^
  --license-tier enterprise

REM Create hardware-bound license
labyrinx-cli.exe --create-license secret.key ^
  --license-customer "Acme Corp" ^
  --license-expiry-days 365 ^
  --license-tier pro ^
  --license-hwid <HWID>

REM Show current machine HWID
labyrinx-cli.exe --license-show-hwid

REM Build standalone License Creator for customers
labyrinx-cli.exe --build-license-creator secret.key ^
  --lc-app-name "My Application"

REM Build standalone HWID tool for customers
labyrinx-cli.exe --build-hwid-tool
```

### 7.4 Restore Command

```batch
REM Restore an obfuscated project to original source
labyrinx-cli.exe -p ./my_project --restore

REM Restore and delete backup
labyrinx-cli.exe -p ./my_project --restore --delete-backup
```

---

## 8. Anti-Debug & Tamper Protection

### 8.1 Anti-Debug Techniques (Level 5+)

Labyrinx employs multiple detection methods at runtime, covering both user-mode and kernel-mode debugging approaches, as well as timing-based detection for step-through debugging.

If a debugger is detected, the application exits immediately without executing your code.

### 8.2 Payload Integrity (LBRX Native)

Every EXE built with the LBRX Native packager includes cryptographic verification:
- **Payload authentication** — the decrypted payload is verified before any files are extracted
- **Per-build random encryption** — every build uses a unique key, cryptographically bound to the payload
- **Runtime module hardening** — runtime files are verified before being loaded; any replacement is detected and blocked

### 8.3 String Encryption

At Level 3+, string constants in your code are encrypted. At higher levels, per-string encryption schemes are randomized. This prevents static analysis tools from finding embedded strings, API keys, URLs, or configuration data.

### 8.4 Ctrl+C / Shutdown Handling (LBRX Native)

The bootloader handles shutdown gracefully:
- **First Ctrl+C**: runs your application's registered shutdown hook (if configured), then exits cleanly
- **Double-tap Ctrl+C**: force-exits immediately for emergency situations
- The shutdown hook lets your app register cleanup code (e.g., save database, close connections)

---

## 9. Code Virtualization

### 9.1 Overview

Level 6 compiles selected functions into a custom stack-based VM bytecode. The function body is replaced with bytecode that is:
- **Randomized** — opcode values vary per build
- **Encrypted** — XOR-encrypted with a per-function key
- **Interpreted** — executed by a VM interpreter embedded in the EXE

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
| Named expressions (`:=`) | Yes |
| Starred expressions (`*args`) | Yes |
| Slice operations | Yes |
| Function calls | Yes |

### 9.3 Dead Code Injection

Between original VM instructions, junk bytecode blocks are inserted using opaque predicates. Decompilation tools produce misleading code paths.

---

## 10. Deep Import Scanning

The LBRX Native builder includes an intelligent dependency resolver that:

1. **Resolves transitive dependencies** — reads package metadata to build the full dependency tree
2. **Deep-scans imports** — AST-scans source files of every resolved package to find undeclared runtime imports, iterating until convergence
3. **Maps package names** — resolves package names to import names via distribution metadata, with fallback heuristics for non-standard naming
4. **Matches sub-packages** — automatically includes internal supporting packages when a parent package is required

This means you can specify a single top-level package and Labyrinx will automatically discover and bundle the full dependency chain — no manual `--hidden-import` flags needed for most projects.

---

## 11. Proven Performance

Labyrinx has been thoroughly tested across a broad range of Python project categories. **100% pass rate across 27 tests in 6 categories** (June 2026).

### Real-World Coverage

The LBRX Native Packager has been verified against:
- **Web frameworks** (multiple WSGI/ASGI servers)
- **ORM and database libraries**
- **Data science and numerical computing toolkits**
- **CLI applications (large-scale, multi-module)**
- **Imaging and graphics libraries**
- **PDF and document generation libraries**
- **Cryptographic toolkits**

All pass at all obfuscation levels from 1 through 6.

### Bootloader Integrity

Multiple tampering vectors were tested. Every attempt was correctly detected and blocked before any application code executed.

### Cleanup & Stability

- **Zero-trace**: no files left on disk after execution (cache disabled)
- **Multi-launch**: consecutive launches produce consistent, correct output
- **Concurrent execution**: multiple instances run without interference

---

## 12. FAQ

**Q: Does obfuscation affect runtime behavior?**  
A: No. Labyrinx transforms your source code's structure, but the runtime behavior is identical. Your existing test suite should pass unchanged.

**Q: Does Labyrinx support async/await code?**  
A: Yes. Async functions are preserved correctly across all protection levels. Level 6 VM does not virtualize async functions (they receive Level 5 protection).

**Q: Can I use Labyrinx in an automated build pipeline?**  
A: Yes. The full CLI interface supports headless automation with JSON output for CI/CD integration.

**Q: What happens if a customer's license expires?**  
A: The application gracefully downgrades to Freemium-level access. The customer can still use the software but with limited protection features.

**Q: Is the license secret safe inside the EXE?**  
A: Yes. The secret is embedded within obfuscated, encrypted modules inside an encrypted payload with per-build random keys. Extracting the secret requires defeating multiple layers of protection.

**Q: Can I rebind a license to a different machine?**  
A: Yes. Generate a new license key without HWID binding, or generate one with the new machine's HWID.

**Q: Can my LBRX Native EXE be unpacked?**  
A: No. LBRX Native uses a proprietary format with per-build encryption. Common extraction tools cannot parse LBRX EXEs.

**Q: What is the minimum EXE size?**  
A: Approximately 29 MB for a minimal application (includes Python runtime, standard library, and Labyrinx runtime). Larger projects scale primarily with their dependencies.

**Q: Does LBRX Native support GUI applications?**  
A: Yes. The GUI runtime is automatically collected and bundled. Both console and windowed modes are supported.

---

## 13. Troubleshooting

### Build Fails at Obfuscation Step
- Ensure your project has no syntax errors (run `python -m py_compile your_file.py` first)
- Check that all imports in your code are resolvable
- Try building with Level 1 first to isolate issues

### Build Fails — Missing Packages
- Use `--hidden-import` to explicitly specify packages that auto-detection misses
- Check that the package is installed in your Python environment (`pip list`)
- The deep import scanner covers most cases; C-extensions with unusual import patterns may need explicit declaration

### EXE Fails to Start (LBRX Native)
- Check the bootloader diagnostic log in the system temp directory for error details
- Verify the Labyrinx installation is complete and runtime files are present
- If an integrity error appears, the EXE may have been corrupted — rebuild

### EXE Fails to Start (License Error)
- Verify the license file is present next to the EXE
- Check the license has not expired
- If HWID-bound, run on the original machine

### Anti-Debug False Positive
- Close all debuggers, IDEs, and monitoring tools
- Check for process monitoring tools (Process Explorer, API Monitor, etc.)
- Build without anti-debug (Level 4 or below) for internal use

### Large EXE Size
- LBRX Native baseline is ~29 MB (Python runtime + standard library + runtime modules)
- Each large dependency adds its own footprint
- Use `--hidden-import` sparingly — the deep import scanner is precise

### Ctrl+C Not Working (LBRX Native)
- The shutdown handler requires a console window. GUI apps (`--noconsole`) use a different shutdown mechanism.
- Set the shutdown hook to run cleanup code on exit

---

## 14. Support

- **Email:** labyrinx@yahoo.com
- **Website:** [labyrinx-dev.github.io](https://labyrinx-dev.github.io/)
- **License key issues:** Include your license key and HWID when contacting support
- **Bug reports:** Include the diagnostic log from the system temp directory and the build output

---

*Labyrinx — Python Code Obfuscation Toolkit*  
*Version 3.2 | (c) 2026 Labyrinx Software*
