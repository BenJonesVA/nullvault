# Webhook Delivery

Fires HTTP callbacks on first access and/or reveal events.

## Supported Formats
- **Discord** — rich embed with visitor details
- **Generic JSON POST** — raw payload to any endpoint

## Functions
- `buildPayload()` — constructs the webhook body
- `fireWebhook()` — sends HTTP POST to `secrets.webhook_url`

## Trigger Events
- First access (first row in `access_logs` for this secret)
- Successful reveal

## Module
[[Modules/webhook.js]]

## Graph Community
Community 21 "Webhook Delivery" — cohesion 1.0, nodes: `buildPayload()`, `fireWebhook()`.
