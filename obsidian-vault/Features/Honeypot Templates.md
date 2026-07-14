# Honeypot Templates

Visual "skins" for the honeypot page. Set via `template` field at creation. Rendered server-side via Nunjucks (`views/secret.njk`).

## Available Templates
| Template | Display Name | Persona |
|----------|-------------|---------|
| `default` | Generic NullVault | Neutral secret link |
| `banking` | SecureDoc Portal | Bank document portal |
| `crypto` | WalletVault | Crypto wallet |
| `invoice` | — | Invoice viewer |
| `corporate` | — | Corporate doc portal |
| `docs` | — | Document viewer |

## Graph Community
Community 18 "Honeypot Templates" — cohesion 0.43, 8 nodes.

## Related
- [[Database/secrets]] — `template` column
- [[Architecture/Route Map]] — `GET /s/:token` renders template
