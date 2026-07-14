# src/middleware/logger.js

Logs every request to `/s/:token*` before the route handler runs.

## What Gets Logged
- IP address (via `geo.extractIP()`)
- Location + org (via `geo.lookupIP()`)
- User agent
- Referer
- Timestamp

## Function
`logAccess(req, secretId)` — called for every matching request, INSERTs into [[Database/access_logs]].

## Position in Chain
Runs BEFORE the route handler, meaning even failed/404 visits are logged.

## Graph Community
Community 19 "Geo & Request Logging" — part of hyperedge: `logger middleware → geolocation → access_logs` (confidence 1.0).

## Related
- [[Modules/geo.js]]
- [[Architecture/Data Flows]]
