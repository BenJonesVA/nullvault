# src/db.js

SQLite schema creation and migrations. Runs on every startup.

## Responsibilities
- Open (or create) SQLite database at `DB_PATH`
- `CREATE TABLE IF NOT EXISTS` for all three tables
- Apply any schema migrations (column additions, index creation)

## Tables Created
- [[Database/secrets]]
- [[Database/access_logs]]
- [[Database/location_beacons]]

## Graph
- Community 30 "SQLite Schema"
- Community 46 "SQLite Schema Community"
