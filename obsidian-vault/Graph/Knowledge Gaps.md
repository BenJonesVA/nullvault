# Knowledge Gaps

Isolated nodes (≤1 connection) and thin communities flagged by graphify.

## Isolated Nodes (48 total)
These have minimal connections — possible missing edges or underdocumented components.

Key ones:
- `src/routes/create.js` — [[Modules/routes/create.js]]
- `src/routes/health.js` — [[Modules/routes/health.js]]
- `src/analysis.js` — [[Modules/analysis.js]]
- `Rate Limiting (20 req/min public, 10 req/min /create)` — [[Features/Rate Limiting]]
- `controlUrl as Password (No Separate Auth System)` — [[Features/Control URL Auth]]

## Thin Communities
Most single-node communities are meta-communities (one concept node per feature area). Low signal — these are graph artifacts from the extraction, not real gaps.

High-priority thin communities to investigate:
- **QR Code Generation** (2 nodes) — `toSVG()` and `qr.js` may have more connections to GPS flow
- **Cleanup Functions** (2 nodes) — `runCleanup()` / `startCleanupScheduler()` connections to `secrets.expires_at` may be missing
- **Webhook Payload Functions** (2 nodes) — `buildPayload()` / `fireWebhook()` likely connect to reveal flow

## Suggested Investigation Questions (from graphify)
1. What is the exact relationship between `NullVault Core Features` and `Scam Baiting and OSINT`? (AMBIGUOUS edge)
2. What connects `src/routes/create.js`, `src/routes/health.js`, `src/analysis.js` to the rest of the system?
3. Should `Chart.js Draw Lifecycle` (cohesion 0.03) be documented as separate modules?
