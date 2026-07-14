# src/cleanup.js

Data retention scheduler. Deletes expired records based on `RETENTION_DAYS` env var.

## Functions
| Function | Purpose |
|----------|---------|
| `runCleanup()` | Delete rows older than retention window |
| `startCleanupScheduler()` | Schedule `runCleanup()` to run on interval |

## Targets
- `access_logs` rows older than `RETENTION_DAYS`
- Expired secrets (`secrets.expires_at < NOW`)

## Graph Community
Community 20 "Data Retention Cleanup" — cohesion 1.0, 2 nodes.

## Related
- [[Database/secrets]] — `expires_at` column
- [[Database/access_logs]]
