# Security Layer

## Components

### Helmet CSP
Configured in `src/app.js`.
- `script-src 'self'` — no inline scripts, no external JS
- `style-src 'self'` — no inline styles, no external CSS
- `default-src 'none'` — everything else blocked

This is why all assets (Chart.js, fonts, maps) are self-hosted in `public/`.

### Rate Limiting
Middleware: [[Modules/middleware/rateLimit.js]]
- Public routes (`/s/*`): 20 req/min
- `/create`: 10 req/min

### Control URL Auth
[[Features/Control URL Auth]] — `controlUrl` token acts as the sole password. No login form, no session. Lose the URL = lose access.

### XOR Obfuscation
[[Features/Link Creation & XOR Obfuscation]] — content stored XOR'd with `public_token` as key. Obfuscation only, not encryption. Anyone with the token can decode.

## Security Hyperedge (inferred, confidence 0.85)
`Helmet CSP` + `rate limiting` + `controlUrl auth` + `XOR obfuscation` form the full security surface.

## Docker
- Runs as non-root `appuser`
- Recommended deployment method

## Related
- [[Architecture/Overview]]
- [[Features/Rate Limiting]]
