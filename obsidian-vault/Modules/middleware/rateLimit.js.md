# src/middleware/rateLimit.js

Rate limiting middleware.

## Limits
- Public routes: 20 req/min
- `/create`: 10 req/min

## Graph Note
Inferred connection: `src/app.js` references `src/rateLimit.js` (confidence not specified — INFERRED edge).

## Related
- [[Features/Rate Limiting]]
- [[Modules/app.js]]
