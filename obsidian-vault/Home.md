# NullVault Knowledge Base

Self-hosted honeypot link tracker for security research and scam baiting.

## Navigation

### Architecture
- [[Architecture/Overview]] — Stack, entry point, middleware chain
- [[Architecture/Route Map]] — All routes and handlers
- [[Architecture/Data Flows]] — Creation → Visit → Reveal → GPS
- [[Architecture/Security Layer]] — Helmet, CSP, rate limiting, XOR

### Database
- [[Database/Schema Overview]] — All tables
- [[Database/secrets]] — Honeypot link records
- [[Database/access_logs]] — Visit records
- [[Database/location_beacons]] — GPS ping records

### Features
- [[Features/Link Creation & XOR Obfuscation]]
- [[Features/Honeypot Templates]]
- [[Features/GPS Tracking]]
- [[Features/Burn on Reveal]]
- [[Features/Webhook Delivery]]
- [[Features/QR Code Generation]]
- [[Features/Rate Limiting]]
- [[Features/Control URL Auth]]

### Modules
- [[Modules/app.js]] — Express entry point
- [[Modules/db.js]] — SQLite schema + migrations
- [[Modules/geo.js]] — IP geolocation
- [[Modules/logger.js]] — Access logging middleware
- [[Modules/analysis.js]] — Access pattern classifier
- [[Modules/cleanup.js]] — Data retention scheduler
- [[Modules/webhook.js]] — Webhook delivery
- [[Modules/qr.js]] — QR code generation
- [[Modules/routes/create.js]]
- [[Modules/routes/secret.js]]
- [[Modules/routes/control.js]]
- [[Modules/routes/health.js]]
- [[Modules/middleware/rateLimit.js]]

### Graph Analysis
- [[Graph/Graph Report]] — 901 nodes, 2329 edges, 62 communities
- [[Graph/God Nodes]] — Most-connected abstractions
- [[Graph/Communities]] — Community hub index
- [[Graph/Surprising Connections]] — Inferred cross-module links
- [[Graph/Knowledge Gaps]] — Isolated nodes / underdocumented components

## Quick Facts
- **Stack:** Express.js + SQLite (better-sqlite3) + Nunjucks
- **No build step** — pure interpreted Node.js
- **Auth:** `controlUrl` token is the password (no separate auth system)
- **Obfuscation:** XOR with `public_token` as key (not encryption)
- **Assets:** 100% self-hosted — fonts, maps, Chart.js, no external CDN
