# src/analysis.js

Access pattern classifier. Analyzes `access_logs` rows to detect behavioral patterns.

## Functions
| Function | Purpose |
|----------|---------|
| `buildNarrativeSummary()` | Generate human-readable summary of access patterns |
| `fmtDatetime()` | Format datetime for display |
| `formatDuration()` | Format duration strings |

## Patterns Detected
- First visit
- Repeat IP
- Rapid revisit
- Multi-device same IP
- (others per implementation)

## Graph Community
Community 16 "Access Pattern Analysis" — cohesion 0.24, 3 nodes.
Community 38 "Access Pattern Community".

## Knowledge Gap Note
Graph flagged `src/analysis.js` as an isolated node (≤1 connection). May have undocumented integrations with control panel display.

## Related
- [[Database/access_logs]]
- [[Modules/routes/control.js]]
