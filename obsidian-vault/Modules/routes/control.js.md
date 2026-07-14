# src/routes/control.js

Control panel route file (separate from `routes/secret.js`).

## Purpose
Serves the private analytics dashboard for a honeypot link. Requires valid `control_token`.

## Graph
- Community 60 "Control.js Route File" — 1 node: `src/routes/control.js`
- Community 32 "Control Panel Route"
- Community 48 "Control Panel Community"

## Note
The CLAUDE.md route map lists control panel under `routes/secret.js`, but the graph also identified `src/routes/control.js` as a separate file. Verify actual file split.

## Related
- [[Features/Control URL Auth]]
- [[Modules/routes/secret.js]]
- [[Modules/analysis.js]]
