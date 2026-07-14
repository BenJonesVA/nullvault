# Burn on Reveal

When `burn_on_reveal=true`, the secret is marked burned after the first successful reveal.

## Behavior
- `secrets.burned` set to `true`
- Subsequent visits to `/s/:token` show burned state (no content)
- Reveal endpoint checks `burned` flag before decoding

## Use Case
One-time view links — content self-destructs after target opens it.

## Related
- [[Database/secrets]] — `burn_on_reveal`, `burned` columns
- [[Modules/routes/secret.js]] — handles burned check
- [[Architecture/Data Flows]] — Reveal Flow
