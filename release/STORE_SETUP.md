# Labyrinx Store — Complete Setup Reference

Version 1.0 | June 2026

---

## Architecture Overview

```
┌──────────────────────────────────────────────────────────────────┐
│                                                                  │
│  GitHub (labyrinx-dev.github.io)                                 │
│  ├── Website (index.md) — product page, pricing, downloads       │
│  └── Release store (release/) — Labyrinx.exe + manual            │
│                                                                  │
│  GitHub (labyrinx-webhook)                                       │
│  └── Webhook server source (deploy_webhook/)                     │
│                                                                  │
│  Render                                                          │
│  └── Webhook server (labyrinx-webhook.onrender.com)              │
│                                                                  │
│  Lemon Squeezy                                                   │
│  ├── Pro product ($9/mo subscription)                            │
│  ├── Enterprise product ($29/mo subscription)                    │
│  └── Webhook → Render server                                     │
│                                                                  │
└──────────────────────────────────────────────────────────────────┘
```

---

## 1. GitHub — Website Repo

**URL:** `https://github.com/Labyrinx-dev/Labyrinx-dev.github.io`  
**Live:** `https://labyrinx-dev.github.io/`  
**Type:** Jekyll (GitHub Pages), theme `jekyll-theme-cayman`

### Files

| File | Purpose |
|------|---------|
| `_config.yml` | Site title, description, theme |
| `index.md` | Main page (pricing, features, downloads, FAQ) |
| `assets/labyrinx_pro.png` | LS product image for Pro |
| `assets/labyrinx_enterprise.png` | LS product image for Enterprise |
| `release/Labyrinx.exe` | Public download (~125 MB, Git LFS) |
| `release/LABYRINX_USER_MANUAL.md` | User manual v3.2 |
| `release/README.md` | Quick start for downloaders |
| `.gitattributes` | Git LFS tracking for `*.exe` |

### Git LFS

```bash
git lfs track "release/*.exe"
```

### Key links on the site

| Link | URL |
|------|-----|
| Free Download | `https://github.com/Labyrinx-dev/Labyrinx-dev.github.io/raw/master/release/Labyrinx.exe` |
| Subscribe Pro | `https://labyrinx.lemonsqueezy.com/checkout/buy/d9731dc3-df7c-441c-b308-310696483a92` |
| Subscribe Enterprise | `https://labyrinx.lemonsqueezy.com/checkout/buy/d97c655e-ebd8-4370-b868-158498d220d3` |
| Contact Email | `labyrinx@yahoo.com` |
| GitHub Org | `https://github.com/Labyrinx-dev` |

---

## 2. GitHub — Webhook Repo

**URL:** `https://github.com/Labyrinx-dev/labyrinx-webhook`  
**Purpose:** Source for the Render webhook server

### Files

| File | Purpose |
|------|---------|
| `ls_webhook_server.py` | Flask webhook server |
| `requirements.txt` | Python dependencies (`flask`) |
| `.gitignore` | Ignore `*.db`, `__pycache__/` |

### Variant ID mapping (in `ls_webhook_server.py`)

```python
LS_VARIANT_TIER = {
    "1125927": "pro",
    "1125914": "enterprise",
}
```

### To update variant IDs

1. Edit `LS_VARIANT_TIER` in `ls_webhook_server.py`
2. Commit + push → Render auto-redeploys

---

## 3. Render — Webhook Server

**URL:** `https://labyrinx-webhook.onrender.com`  
**Plan:** Free tier

### Render settings

| Setting | Value |
|---------|-------|
| **Name** | `labyrinx-webhook` |
| **Runtime** | Python 3 |
| **Repo** | `https://github.com/Labyrinx-dev/labyrinx-webhook` |
| **Build Command** | `pip install -r requirements.txt` |
| **Start Command** | `python ls_webhook_server.py --port $PORT` |

### Environment Variables

| Key | Value | Notes |
|-----|-------|-------|
| `LICENSE_SECRET` | `BIrEvL5PsFhGL8f5pYk7rw==` | Base64 of `secret/license.key` |
| `LS_WEBHOOK_SECRET` | `90af3bce7baf9eeab5912f8542ef232df8e995f9` | LS signing secret |

### Endpoints

| Path | Purpose | Auth |
|------|---------|------|
| `POST /webhook/ls` | LS sends subscription events here | HMAC signature |
| `GET /health` | Health check | None |
| `GET /licenses` | List all generated licenses | None (protect later) |
| `GET /license/<order_id>` | Lookup license by LS order ID | None (protect later) |

### Logs

- Render dashboard → `labyrinx-webhook` → Logs
- SQLite database: `/opt/render/project/src/ls_orders.db` (Render disk)
  - Table `orders` — all generated licenses
  - Table `webhook_log` — all webhook events received

### Restart

Push to `labyrinx-webhook` GitHub repo → Render auto-redeploys.

---

## 4. Lemon Squeezy

**URL:** `https://app.lemonsqueezy.com`  
**Store:** `labyrinx.lemonsqueezy.com`

### Products

| | Pro | Enterprise |
|---|---|---|
| **Type** | Subscription | Subscription |
| **Price** | $9.00/month | $29.00/month |
| **Variant ID** | `1125927` | `1125914` |
| **Checkout URL** | `/checkout/buy/d9731dc3-...` | `/checkout/buy/d97c655e-...` |
| **Image** | `assets/labyrinx_pro.png` | `assets/labyrinx_enterprise.png` |
| **Free Trial** | 0 days | 0 days |
| **Setup Fee** | $0 | $0 |
| **Metered** | No | No |

### Webhook Settings

| Setting | Value |
|---------|-------|
| **Callback URL** | `https://labyrinx-webhook.onrender.com/webhook/ls` |
| **Signing Secret** | `90af3bce7baf9eeab5912f8542ef232df8e995f9` |
| **Events** | `subscription_created`, `subscription_payment_success`, `subscription_cancelled`, `subscription_expired` |

### Webhook flow

| Event | Server action |
|-------|--------------|
| `subscription_created` | Generate new license key, store in DB |
| `subscription_payment_success` | Renew license key (update expiry) |
| `subscription_cancelled` | Log — key stays valid until expiry |
| `subscription_expired` | Log — no immediate revocation |

---

## 5. Local Development — Secret Key

**Location:** `Labyrinx PyObfuscator\secret\license.key`  
**Size:** 16 bytes  
**Base64:** `BIrEvL5PsFhGL8f5pYk7rw==`

This is the **single source of truth**. All license keys are generated from this secret.

### Where it's used

| Component | How | Location |
|-----------|-----|----------|
| CLI/GUI build | Embedded during obfuscation | `_license_secret.key` (copy) |
| Render webhook | `LICENSE_SECRET` env var (base64) | Render dashboard |
| Manual license creation | `--create-license` flag | CLI tool |

### Backup

**Keep a secure offline backup.** If this secret is lost, all previously-issued license keys become unverifiable.

### Generate a new secret (if needed)

```powershell
.\labyrinx-cli.exe --license-gen-secret secret/license.key
python -c "import base64; print('LICENSE_SECRET=' + base64.b64encode(open('secret/license.key','rb').read()).decode())"
```

Then update:
1. Render → `LICENSE_SECRET` env var
2. Rebuild all distributed EXEs (they embed the old secret)

---

## 6. Customer Flow

```
1. Customer visits labyrixx-dev.github.io
2. Downloads Labyrinx.exe (free, Freemium mode)
3. Tests the product at Level 1-2
4. Clicks "Subscribe — Pro" or "Enterprise"
5. Lemon Squeezy processes payment
6. LS sends webhook to Render server
7. Render server generates license key
8. LS includes license key in receipt email
9. Customer opens Labyrinx.exe, pastes license key
10. Pro/Enterprise features unlocked
```

---

## 7. Support & Contact

| Channel | Detail |
|---------|--------|
| Email | `labyrinx@yahoo.com` |
| Website | `https://labyrinx-dev.github.io/` |
| GitHub | `https://github.com/Labyrinx-dev` |
| LS Store | `https://labyrinx.lemonsqueezy.com` |
| License lookup | `GET https://labyrinx-webhook.onrender.com/licenses` |
| Health check | `GET https://labyrinx-webhook.onrender.com/health` |

---

## 8. Rebuild Checklist

When rebuilding for a new release:

- [ ] Build bootloaders (`bootloader_console.exe`, `bootloader_windows.exe`)
- [ ] Build CLI Release (LBRX, persistent mode) — Admin tool → Build CLI (Release)
- [ ] Build GUI (LBRX) — Admin tool → Build GUI
- [ ] Copy `Labyrinx.exe` to `release/` folder → `git push`
- [ ] Update user manual if needed → copy to `release/` → `git push`
- [ ] Test download link works
- [ ] Test webhook server health check
- [ ] Do a test purchase through LS → verify license key generated
- [ ] Verify license key activates in `Labyrinx.exe`
