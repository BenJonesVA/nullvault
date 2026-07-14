# God Nodes

Most-connected nodes in the graph — core abstractions.

## Top 10
| Rank | Node | Edges | Note |
|------|------|-------|------|
| 1 | `tn` | 124 | Chart.js internal — cross-community bridge |
| 2 | `n()` | 59 | Chart.js internal |
| 3 | `js` | 58 | Chart.js internal |
| 4 | `s()` | 31 | Chart.js internal |
| 5 | `o()` | 30 | Chart.js internal |
| 6 | `ho()` | 30 | Chart.js internal |
| 7 | `wa` | 28 | Chart.js internal |
| 8 | `updateElements()` | 25 | Chart.js internal |
| 9 | `a()` | 24 | Chart.js internal |
| 10 | `l()` | 24 | Chart.js internal |

## Observation
All top god nodes are Chart.js library internals from the minified `public/chart.min.js`. The NullVault application code itself has no single god node — architecture is well-distributed.

## Bridge Node
`tn` (betweenness centrality 0.131) bridges: Chart.js Core Internals ↔ Draw Lifecycle ↔ Scale & Tick Building ↔ Update Pipeline ↔ Data Limits ↔ Event Handlers ↔ Plugin System ↔ Draw Events ↔ Element Management ↔ Canvas Context.
