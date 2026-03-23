# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

NullVault is a self-hosted honeypot link tracker for security research and scam baiting. Users create deceptive URLs that log who visits, what device/location they use, and optionally prompt for GPS. The app is strictly educational/research — no credential harvesting, no doxxing.

## Commands

```bash
# Development (file watcher)
npm run dev

# Production
npm start

# Docker (recommended deployment)
docker compose up -d --build   # Build and start
docker compose logs -f          # Follow logs
```

No test framework exists in this project.

## Environment Setup

Copy `.env.example` to `.env`. Two variables are required:

```env
BASE_URL=https://your-domain.com   # No trailing slash
CREATION_SECRET=<long-random-hex>  # Auth token for POST /create
```

All other variables (PORT, DB_PATH, RETENTION_DAYS, etc.) have defaults defined in `src/app.js`.

## Architecture

**Stack:** Express.js + SQLite (better-sqlite3) + Nunjucks templates. No build step — pure interpreted Node.js. All assets (fonts, maps, charts) are self-hosted in `public/`.

### Route Map

| Route | Handler | Purpose |
|-------|---------|---------|
| `POST /create` | `routes/create.js` | Create honeypot link (requires `X-Creation-Secret` header) |
| `GET /s/:token` | `routes/secret.js` | Public honeypot page (victim-facing) |
| `POST /s/:token/reveal` | `routes/secret.js` | Deobfuscate and log reveal event |
| `GET /s/:token/control` | `routes/secret.js` | Private analytics dashboard |
| `POST /s/:token/webhook` | `routes/secret.js` | Save webhook URL |
| `GET /health` | `routes/health.js` | Health check |

### Data Flow

1. **Creation** → random `public_token` + `control_token` generated → secret content XOR-obfuscated with `public_token` as key → stored in SQLite
2. **Visit** → `middleware/logger.js` intercepts every request to `/s/:token*` → logs IP, UA, referrer, geolocation (`geo.js`) to `access_logs`
3. **Reveal** → client POSTs to `/reveal` → server XOR-decodes content → if `burn_on_reveal=true`, marks secret burned
4. **GPS** → if `require_location=true`, `public/secret.js` prompts browser geolocation → beacons stored in `location_beacons` table
5. **Webhooks** → `webhook.js` fires Discord embeds or generic JSON POST on first access and/or reveal events

### Key Files

- `src/app.js` — Express setup, Helmet CSP config, Nunjucks init, middleware/route mounting
- `src/db.js` — SQLite schema creation and any migrations (runs on startup)
- `src/routes/secret.js` — Most complex route file; handles both public honeypot and control panel logic
- `src/middleware/logger.js` — Logs every access with geolocation before routes run
- `src/analysis.js` — Classifies access patterns (first visit, repeat IP, rapid revisit, etc.)
- `public/secret.js` — Client-side reveal flow and GPS beacon logic
- `public/control.js` — Dashboard interactivity (Chart.js charts, map highlighting, tabs)

### Database Schema (SQLite)

**`secrets`** — one row per honeypot link: `public_token`, `control_token`, `content` (XOR-obfuscated), `template`, `webhook_url`, `burn_on_reveal`, `require_location`, `burned`, `expires_at`

**`access_logs`** — one row per visit: `secret_id`, `ip_address`, `location`, `org`, `user_agent`, `referer`, `reveal_attempted`, `reveal_succeeded`, `gps_lat`, `gps_lng`

**`location_beacons`** — GPS pings from mobile location sharing: `secret_id`, `gps_lat`, `gps_lng`, `accuracy`

### Security Constraints

- Helmet enforces strict CSP: `script-src 'self'`, `style-src 'self'`, `default-src 'none'` — no inline scripts or external resources allowed
- XOR obfuscation uses `public_token` as the symmetric key — it's obfuscation, not encryption
- `controlUrl` acts as the password; there is no separate auth system
- Rate limiting: 20 req/min on public routes, 10 req/min on `/create`
- Docker runs as non-root `appuser`

### Templates

Honeypot pages support visual "skins" via the `template` field: `default`, `banking`, `crypto`, `invoice`, `corporate`, `docs`. Templates are rendered server-side via Nunjucks (`views/secret.njk`).
