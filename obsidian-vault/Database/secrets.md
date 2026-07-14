# DB Table: secrets

One row per honeypot link.

## Columns
| Column | Type | Notes |
|--------|------|-------|
| `id` | INTEGER PK | |
| `public_token` | TEXT | Unique; used as XOR key for content |
| `control_token` | TEXT | Unique; forms the `controlUrl` password |
| `content` | TEXT | XOR-obfuscated secret content |
| `template` | TEXT | Skin: `default`, `banking`, `crypto`, `invoice`, `corporate`, `docs` |
| `webhook_url` | TEXT | Discord embed or generic JSON POST target |
| `burn_on_reveal` | BOOLEAN | Delete/mark burned after first successful reveal |
| `require_location` | BOOLEAN | Force GPS prompt before reveal |
| `burned` | BOOLEAN | Set true after burn-on-reveal fires |
| `expires_at` | DATETIME | Cleanup scheduler uses this |

## Related
- [[Features/Link Creation & XOR Obfuscation]]
- [[Features/Burn on Reveal]]
- [[Features/GPS Tracking]]
- [[Features/Honeypot Templates]]
- [[Modules/db.js]]
