# Link Creation & XOR Obfuscation

## Creation Steps
1. Generate random `public_token` and `control_token`
2. XOR-encode `content` byte-by-byte using `public_token` as repeating key
3. INSERT into [[Database/secrets]]
4. Return `secretUrl = BASE_URL/s/<public_token>` and `controlUrl = BASE_URL/s/<public_token>/control?t=<control_token>`

## XOR Obfuscation
- Symmetric: same operation encodes and decodes
- Key: `public_token` (the URL slug itself)
- **Not encryption** — anyone with the URL can decode; purpose is to avoid plaintext in DB
- Stored in `secrets.content`

## Reveal
Client POSTs to `/s/:token/reveal` → server XOR-decodes → returns plaintext.

## Handler
[[Modules/routes/create.js]] — requires `X-Creation-Secret` header matching `CREATION_SECRET` env var.

## Graph Community
Graph community "Link Creation & XOR Obfuscation" (Community 22) — also part of hyperedge: `token generation → XOR obfuscation → SQLite storage` (confidence 1.0).
