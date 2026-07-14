# Surprising Connections

Inferred edges the graph found that aren't in explicit documentation.

## Extracted (confidence 1.0)
| From | Relation | To |
|------|----------|----|
| `public/us-map.svg` | references | Natural Earth + US Atlas TopoJSON (SVG Source Data) |

## Inferred (model-generated)
| From | Relation | To | Confidence |
|------|----------|----|-----------|
| `public/us-map.svg` | shares_data_with | `DB Table: access_logs` | inferred |
| `public/world-map.svg` | shares_data_with | `DB Table: access_logs` | inferred |
| `src/app.js` | references | `src/rateLimit.js` | inferred |

## Interpretation
- The SVG maps in the control panel visualize visitor location data from `access_logs` — the graph inferred this cross-module dependency correctly even without explicit code references.
- `app.js → rateLimit.js` is an undocumented import the graph caught.

## Ambiguous
| Edge | Relation | Note |
|------|----------|------|
| `NullVault Core Features` → `Scam Baiting and OSINT` | conceptually_related_to | AMBIGUOUS — tagged for review |
