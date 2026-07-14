# src/webhook.js

Webhook delivery module. Fires on first access and/or reveal events.

## Functions
| Function | Purpose |
|----------|---------|
| `buildPayload(event, data)` | Build Discord embed or generic JSON body |
| `fireWebhook(url, payload)` | HTTP POST to `secrets.webhook_url` |

## Graph Community
Community 21 "Webhook Delivery" — cohesion 1.0.
Community 26 "Webhook Payload Functions".
Community 42 "Webhook Community".

## Related
- [[Features/Webhook Delivery]]
- [[Database/secrets]] — `webhook_url` column
