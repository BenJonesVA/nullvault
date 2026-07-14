# Rate Limiting

Express middleware limiting request rates to prevent abuse.

## Limits
| Route | Limit |
|-------|-------|
| Public routes (`/s/*`) | 20 req/min |
| `/create` | 10 req/min |

## Module
[[Modules/middleware/rateLimit.js]]

## Graph
- Community 31 "Rate Limiting Middleware"
- Community 47 "Rate Limiting Community"
- Community 50 "Rate Limiting Feature"
- Listed in [[Architecture/Security Layer]] hyperedge
