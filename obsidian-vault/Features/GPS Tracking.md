# GPS Tracking

## Flow
```
require_location=true on secret
  → GET /s/:token renders page with location flag
  → public/secret.js: navigator.geolocation.getCurrentPosition()
    → Granted: beacon lat/lng to server → location_beacons table
    → Denied: show QR code for mobile scan (see QR Code Generation)
```

## Two GPS capture mechanisms
1. **One-shot** — `access_logs.gps_lat / gps_lng` populated at reveal time from browser geolocation
2. **Beacons** — repeated pings stored in `location_beacons` table (ongoing mobile tracking)

## Relevant Files
- `public/secret.js` — client-side prompt + beacon logic
- [[Database/location_beacons]]
- [[Features/QR Code Generation]] — fallback when desktop browser denies location

## Graph Hyperedge
`require_location → browser prompt → location_beacons` (confidence 1.0, extracted)
