# Architecture Overview

## Stack
| Layer | Technology |
|-------|-----------|
| HTTP server | Express.js |
| Database | SQLite via `better-sqlite3` |
| Templates | Nunjucks (server-side) |
| Security headers | Helmet |
| Geolocation | GeoIP-Lite |
| Charts | Chart.js (self-hosted) |
| Maps | US + World SVG (self-hosted) |

## Entry Point
`src/app.js` — initializes Express, configures Helmet CSP, mounts Nunjucks, loads middleware and routes.

## Middleware Chain (every `/s/:token*` request)
1. Rate limiter (`src/middleware/rateLimit.js`)
2. Access logger (`src/middleware/logger.js`) — IP, UA, referrer, geolocation → `access_logs`
3. Route handler

## Key Design Decisions
- **No build step** — files served as-is, Nunjucks rendered server-side
- **Self-hosted assets** — no external requests; CSP enforces `default-src 'none'`
- **XOR obfuscation** — content stored encoded, decoded on reveal; `public_token` is the key
- **controlUrl as auth** — guessable-length token acts as the sole password

## Related
- [[Route Map]]
- [[Data Flows]]
- [[Security Layer]]
- [[Modules/app.js]]
