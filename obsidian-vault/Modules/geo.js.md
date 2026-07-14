# src/geo.js

IP geolocation using GeoIP-Lite.

## Functions
| Function | Purpose |
|----------|---------|
| `extractIP(req)` | Pull real IP from request, handle proxies/X-Forwarded-For |
| `lookupIP(ip)` | Query GeoIP-Lite → `{ location, org }` |

## Output
- `location` — "City, Country" string → stored in `access_logs.location`
- `org` — ASN/org string → stored in `access_logs.org`

## Graph Community
Community 19 "Geo & Request Logging" — cohesion 0.5, nodes: `lookupIP()`, `extractIP()`, `logAccess()`.

## Related
- [[Modules/logger.js]]
- [[Database/access_logs]]
