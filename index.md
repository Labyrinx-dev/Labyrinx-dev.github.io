---
layout: default
---

# Labyrinx — AI Code Protection

{: .text-center}
**Safeguard your ML intellectual property. 7 layers of protection.**

---

## 🔒 What Labyrinx Does

Turn your Python source code into a protected, standalone Windows executable — with military-grade obfuscation.

| Layer | Protection |
|-------|-----------|
| 1. Name Obfuscation | All variables, functions, classes renamed to random identifiers |
| 2. String Encryption | Every string literal packed into XOR+base85 encrypted blob |
| 3. Control Flow Flattening | All function logic flattened into switch-case state machines |
| 4. Dead Code Injection | Opaque predicates inject junk code blocks between real statements |
| 5. Anti-Debug (×6) | Debugger detection via gettrace, IsDebuggerPresent, DebugPort, NtGlobalFlag, timing, hardware breakpoints |
| 6. Module Encryption | AES-256-CTR encrypts entire `.py` modules — decrypted only in memory |
| 7. Code Virtualization | 49-opcode custom stack VM replaces Python bytecode entirely |

{: .text-center}
*No other Python obfuscator ships a code VM.*

---

## 📦 How It Works

```
Your .py files  →  Labyrinx  →  Protected .exe
                    (7 layers)      (standalone, no Python needed)
```

1. **Open Labyrinx** — point it at your Python project
2. **Choose protection level** — from 1 (names only) to 6 (full VM)
3. **Click Build** — Labyrinx obfuscates every `.py` file and packages an `.exe`
4. **Distribute** — single `.exe` with built-in license key system

{: .text-center}
*Tested against 11 major open-source Python projects*

---

## 💰 Pricing

| | Freemium | Pro | Enterprise |
|---|---------|-----|------------|
| **Price** | Free | $9/month | $29/month |
| **Max Level** | 2 (names) | 4 (strings, CF) | 6 (VM, anti-debug) |
| **EXE Bundling** | ✅ Single-file | ✅ Full project | ✅ Full project |
| **String Encryption** | — | ✅ | ✅ |
| **Module Encryption** | — | — | ✅ AES-256-CTR |
| **Anti-Debug** | — | — | ✅ ×6 techniques |
| **Code VM** | — | — | ✅ 49-opcode |
| **License System** | — | ✅ | ✅ |
| **Installer Builder** | — | ✅ | ✅ |

{: .text-center}
*All tiers include lifetime access. No per-build fees. No royalties.*

---

## 🚀 Getting Started

1. Download Labyrinx (coming soon)
2. Open your Python project
3. Click Build

{: .text-center}
*Contact us at [your-email] for early access and enterprise inquiries.*

---

## ❓ FAQ

**Q: Does it work with Flask, Django, FastAPI?**
A: Yes. Tested with Flask, FastAPI, Typer, Rich, Textual, Pydantic, Starlette, and more.

**Q: Does the protected EXE need Python installed?**
A: No. The EXE is completely standalone — includes the Python runtime.

**Q: Can I sell software protected with Labyrinx?**
A: Yes. The built-in HMAC license system lets you generate and distribute license keys to your customers.

**Q: Is my source code still readable?**
A: At Level 6, the original Python bytecode is replaced with custom VM bytecode. Even if extracted, what an attacker sees is:
```
PUSH_CONST 142  →  PUSH_FAST 3  →  BINOP_ADD  →  CALL 2  →  ...
```

---

{: .text-center .small}
© 2026 Labyrinx. All rights reserved.
