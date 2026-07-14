# Route Map

| Method | Route | Handler | Purpose |
|--------|-------|---------|---------|
| `POST` | `/create` | `routes/create.js` | Create honeypot link — requires `X-Creation-Secret` header |
| `GET` | `/s/:token` | `routes/secret.js` | Public honeypot page (victim-facing) |
| `POST` | `/s/:token/reveal` | `routes/secret.js` | Deobfuscate + log reveal event |
| `GET` | `/s/:token/control` | `routes/secret.js` | Private analytics dashboard |
| `POST` | `/s/:token/webhook` | `routes/secret.js` | Save webhook URL |
| `GET` | `/health` | `routes/health.js` | Health check |

## Notes
- All `/s/:token*` routes pass through [[Modules/logger.js]] before the handler runs
- `/create` has its own tighter rate limit (10 req/min vs 20 for public routes)
- Control panel route lives in `routes/secret.js`, not a separate file — see also [[Modules/routes/control.js]]

## Related
- [[Architecture/Data Flows]]
- [[Features/Control URL Auth]]
- [[Features/Rate Limiting]]
