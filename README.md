# 🔒 NullVault

A self-hosted honeypot link tracker. Create a secret-looking URL, share it with a target, and get notified the instant it's opened — with geolocation, device info, a private control panel, and optional webhook alerts.

Built for scam baiting, OSINT, and catching unauthorized access to sensitive material.

> **Built entirely with [Claude](https://claude.ai) (Anthropic).** Every line of code, config, and documentation in this project was written through an iterative AI-assisted development session — no manual coding.

**[Watch the setup & feature walkthrough on YouTube](https://youtu.be/lQNXXxNvbOo)**

---

## Features

- **Deceptive templates** — the public page looks like a generic secret share, a banking portal, a crypto wallet, an invoice, a corporate document, or a docs site
- **Burn-on-reveal toggle** — optionally destroy the secret after the first read, or leave it persistent for continued monitoring
- **Full access logging** — every visit captures IP, city/state/country, ISP, browser, OS, referrer, and timestamp
- **Private control panel** — stat cards, activity chart, world map, US state heat map, device breakdown, IP lookup, raw log table with CSV export
- **Webhook alerts** — configure a Discord webhook or any HTTP endpoint from the control panel; test pings included
- **Location tracking** — optionally require GPS confirmation before revealing the secret; mobile visitors are prompted to share location
- **Zero external requests at runtime** — fonts, Chart.js, and map SVGs are all self-hosted; no CDN calls, no third-party analytics
- **Strict CSP** — `script-src 'self'`, `style-src 'self'`, `default-src 'none'`; zero inline styles or scripts
- **Docker-first** — single `docker compose up -d` to deploy; SQLite persisted in a named volume

---

## Quick Start

### 1. Clone and configure

```bash
git clone https://github.com/rehatiel/nullvault.git
cd nullvault
cp .env.example .env
```

Open `.env` and set at minimum:

```env
BASE_URL=https://your-domain.com
CREATION_SECRET=your-long-random-secret
```

Generate a strong secret:

```bash
openssl rand -hex 32
```

### 2. Deploy

```bash
docker compose up -d
```

The app listens on port `3000`. Put Nginx, Caddy, or a Cloudflare Tunnel in front.

Visit `https://your-domain.com` to create your first honeypot link from the web UI.

---

## Usage

### Web UI

Visit the home page to create a link using the form. Options:

| Field | Description |
|---|---|
| **Content** | The secret text the visitor sees on reveal (up to 4,096 chars) |
| **Template** | Visual decoy skin shown to the visitor |
| **Expiry** | Days until the link stops working (`0` = never) |
| **Burn after reading** | Destroy the secret after the first successful reveal |
| **Require location** | Prompt the visitor for GPS before revealing the secret |

After creation you'll receive a **public URL** to share and a **control URL** to bookmark — the control URL is your private dashboard.

### Templates

| Value | Looks like |
|---|---|
| `default` | Generic NullVault secure-secret page |
| `banking` | "SecureDoc Portal" — corporate document delivery |
| `crypto` | "WalletVault" — crypto wallet seed-phrase viewer |
| `invoice` | Invoice document viewer |
| `corporate` | Corporate internal document portal |
| `docs` | Generic documentation/file share |

### Control Panel

Every link has a private dashboard at `/s/<public_token>/control`:

| Tab | Contents |
|---|---|
| **Overview** | Stat cards, recent activity timeline, world map, US state heat map |
| **Timeline** | Full chronological log with event type, IP, location |
| **Devices** | Browser and OS breakdown from User-Agent strings |
| **IP Addresses** | Deduplicated IPs with location, ISP, and IPInfo lookup link |
| **Raw Logs** | Full table with CSV export |
| **Settings** | Webhook URL, location requirement toggle — set, update, clear, or test any time |

### Maps

- **World map** — countries with accesses are highlighted; hover for name + count
- **US state heat map** — appears automatically when US traffic is detected; states coloured on a 4-level scale (amber → red) relative to the most-active state

### Webhook Alerts

Configure from the **Settings** tab. Format is auto-detected:

- **Discord** — rich embed with event type, IP, location, user agent, referrer
- **Generic HTTP** — JSON POST with `event`, `publicUrl`, `ip`, `location`, `userAgent`, `referer`, `timestamp`

A **Send Test Ping** button fires a real request with `test: true` to verify delivery.

---

## API

All endpoints return JSON. Link creation requires an `X-Creation-Secret` header matching `CREATION_SECRET`.

### `POST /create`

**Headers:**
```
X-Creation-Secret: <CREATION_SECRET>
Content-Type: application/json
```

**Body:**

| Field | Type | Required | Notes |
|---|---|---|---|
| `content` | string | ✅ | Up to 4,096 chars |
| `template` | string | | `default`, `banking`, `crypto`, `invoice`, `corporate`, `docs` |
| `expiryDays` | number | | `0` = never; defaults to `DEFAULT_EXPIRY_DAYS` |
| `burnOnReveal` | boolean | | `true` = one-time reveal |

**Response `201`:**
```json
{
  "publicUrl":  "https://your-domain.com/s/<token>",
  "controlUrl": "https://your-domain.com/s/<token>/control",
  "expiresAt":  1234567890
}
```

### Other endpoints

| Method | Path | Description |
|---|---|---|
| `GET` | `/s/:token` | Public honeypot page |
| `POST` | `/s/:token/reveal` | Reveal secret (called by public page) |
| `GET` | `/s/:token/control` | Private control panel |
| `POST` | `/s/:token/webhook` | Update webhook URL (`{ "webhookUrl": "..." }` or `null` to clear) |
| `POST` | `/s/:token/webhook/test` | Fire a test ping |
| `GET` | `/health` | Returns `{ "status": "ok" }` |

---

## Configuration

| Variable | Default | Description |
|---|---|---|
| `BASE_URL` | — | **Required.** Full public URL, no trailing slash |
| `CREATION_SECRET` | — | **Required.** `X-Creation-Secret` header value for `/create` |
| `PORT` | `3000` | Listen port |
| `TRUST_PROXY` | `1` | Express trust-proxy depth |
| `DEFAULT_EXPIRY_DAYS` | `30` | Default link lifetime in days |
| `RETENTION_DAYS` | `30` | Access log retention in days |
| `RATE_LIMIT_WINDOW_MS` | `60000` | Rate limit window (ms) |
| `RATE_LIMIT_MAX` | `20` | Max requests per window per IP |
| `DB_PATH` | `/app/data/honeypot.db` | SQLite path inside container |

---

## Reverse Proxy

### Nginx

```nginx
server {
    listen 443 ssl;
    server_name your-domain.com;

    location / {
        proxy_pass         http://localhost:3000;
        proxy_set_header   Host              $host;
        proxy_set_header   X-Real-IP         $remote_addr;
        proxy_set_header   X-Forwarded-For   $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

### Caddy

```
your-domain.com {
    reverse_proxy localhost:3000
}
```

### Cloudflare Tunnel

Set `TRUST_PROXY=1`. NullVault reads the real IP from `CF-Connecting-IP` automatically.

---

## Updating

```bash
git pull
docker compose up -d --build
```

The database schema migrates automatically on startup — no manual steps needed.

---

## Security Notes

- The `controlUrl` is the only access control for the dashboard — treat it like a password
- `CREATION_SECRET` protects link creation; use a long random value
- Geolocation uses the bundled `geoip-lite` offline database — no outbound IP lookups
- Helmet.js enforces strict CSP: no inline scripts, no inline styles, no external resources
- SQLite WAL mode is enabled for safe concurrent reads during traffic spikes

---

## Stack

| Layer | Technology |
|---|---|
| Runtime | Node.js 20 (Alpine) |
| Web framework | Express 4 |
| Database | SQLite via `better-sqlite3` |
| Templates | Nunjucks |
| Geolocation | `geoip-lite` (fully offline) |
| Security headers | Helmet.js |
| Maps | Natural Earth + US Atlas TopoJSON (pre-rendered SVG) |
| Charts | Chart.js 4 (self-hosted) |
| Fonts | IBM Plex Sans/Mono (self-hosted WOFF2) |

---

## Data Collected

This project intentionally limits data collection. Logged metadata includes:

- IP address, timestamp, and requested path
- User-Agent string and HTTP referrer
- Standard HTTP headers (`Accept-Language`, `Sec-CH-UA`, `Sec-Fetch-Site`)
- Approximate geolocation (city / region / country via offline database)
- GPS coordinates (only if the visitor explicitly grants browser location permission)

The application does not execute client-side exploits, use invasive fingerprinting, track users across sessions, or attempt to deanonymize visitors.

---

## ⚠️ Important Disclaimer

This software is provided for **educational and research purposes only**.

By using this project, you agree that:

- You are responsible for complying with all applicable local, state, and international laws
- You will not use this software for harassment, doxxing, retaliation, or unauthorized surveillance
- You understand that IP-based geolocation is approximate and unreliable
- You will not attempt to identify, track, or harm individuals using this software
- The authors and contributors assume no liability for misuse

**This is not** a hacking tool, a doxxing platform, a credential harvester, or a law enforcement solution.

---

## License

MIT
