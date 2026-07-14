# DB Table: location_beacons

GPS pings from mobile location sharing. Separate from `access_logs.gps_lat/lng` which captures one-shot browser geolocation — beacons are repeated pings from ongoing mobile tracking.

## Columns
| Column | Type | Notes |
|--------|------|-------|
| `id` | INTEGER PK | |
| `secret_id` | INTEGER FK | → `secrets.id` |
| `gps_lat` | REAL | |
| `gps_lng` | REAL | |
| `accuracy` | REAL | Meters, from browser Geolocation API |
| `created_at` | DATETIME | |

## Flow
1. `require_location=true` on secret
2. `public/secret.js` prompts browser geolocation
3. On approval, beacons POST at intervals → stored here
4. If denied, [[Features/QR Code Generation]] serves a QR for mobile scan

## Related
- [[Features/GPS Tracking]]
- [[Database/secrets]]
