# src/routes/secret.js

Most complex route file. Handles both public honeypot pages and control panel.

## Routes Handled
| Route | Purpose |
|-------|---------|
| `GET /s/:token` | Render honeypot template |
| `POST /s/:token/reveal` | Decode + log reveal |
| `GET /s/:token/control` | Analytics dashboard (auth via `control_token`) |
| `POST /s/:token/webhook` | Save webhook URL |

## Key Logic
- Checks `secrets.burned` before serving content
- Handles `require_location` flag → passes to template for client-side GPS prompt
- Updates `access_logs.reveal_attempted` / `reveal_succeeded`
- Calls [[Modules/webhook.js]] on trigger events
- Calls [[Modules/analysis.js]] to build narrative for control panel

## Related
- [[Features/Burn on Reveal]]
- [[Features/GPS Tracking]]
- [[Features/Control URL Auth]]
- [[Architecture/Data Flows]]
