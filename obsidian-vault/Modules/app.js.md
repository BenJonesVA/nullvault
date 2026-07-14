# src/app.js

Express entry point.

## Responsibilities
- Initialize Express app
- Configure Helmet with strict CSP (`script-src 'self'`, `style-src 'self'`, `default-src 'none'`)
- Initialize Nunjucks template engine pointing at `views/`
- Mount global middleware (rate limiter, logger)
- Mount route handlers
- Set default env vars (PORT, DB_PATH, RETENTION_DAYS, etc.)

## Related
- [[Architecture/Overview]]
- [[Architecture/Security Layer]]
- [[Modules/middleware/rateLimit.js]]
- [[Modules/logger.js]]

## Graph
- Community 29 "Express App Entry"
- Community 45 "Express Entry Community"
- God node cross-reference: `src/app.js` inferred to reference `src/rateLimit.js`
