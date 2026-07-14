# DB Table: access_logs

One row per visit or reveal event on any honeypot link.

## Columns
| Column | Type | Notes |
|--------|------|-------|
| `id` | INTEGER PK | |
| `secret_id` | INTEGER FK | → `secrets.id` |
| `ip_address` | TEXT | Raw IP (after proxy extraction) |
| `location` | TEXT | City/country from GeoIP-Lite |
| `org` | TEXT | ASN/org from GeoIP-Lite |
| `user_agent` | TEXT | Full UA string |
| `referer` | TEXT | HTTP Referer header |
| `reveal_attempted` | BOOLEAN | Client POSTed to `/reveal` |
| `reveal_succeeded` | BOOLEAN | Decode succeeded |
| `gps_lat` | REAL | From browser geolocation (if granted) |
| `gps_lng` | REAL | From browser geolocation (if granted) |
| `created_at` | DATETIME | |

## Populated By
- [[Modules/logger.js]] — every request to `/s/:token*`
- [[Modules/routes/secret.js]] — updates `reveal_attempted` / `reveal_succeeded`

## Visualized By
- Control panel: Chart.js activity chart, world map heat map, US state heat map
- [[Modules/analysis.js]] — classifies patterns across rows for this secret
