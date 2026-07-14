# Database Schema Overview

SQLite via `better-sqlite3`. Schema created + migrated in [[Modules/db.js]] on startup.

## Tables

| Table | Purpose |
|-------|---------|
| [[Database/secrets]] | One row per honeypot link |
| [[Database/access_logs]] | One row per visit/reveal event |
| [[Database/location_beacons]] | GPS pings from mobile location sharing |

## Relationships
```
secrets.id
  ├── access_logs.secret_id  (many visits per secret)
  └── location_beacons.secret_id  (many GPS pings per secret)
```

## Surprising Graph Connection
- `public/us-map.svg` and `public/world-map.svg` both `shares_data_with` `access_logs` — they visualize the `location` column on the control panel dashboard.
