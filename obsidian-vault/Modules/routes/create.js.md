# src/routes/create.js

Handles honeypot link creation.

## Route
`POST /create`

## Auth
Requires `X-Creation-Secret` header matching `CREATION_SECRET` env var.

## Process
1. Validate request body (content, template, options)
2. Generate `public_token` + `control_token`
3. XOR-encode content
4. INSERT into `secrets`
5. Return `{ secretUrl, controlUrl }`

## Graph Note
Flagged as isolated node (≤1 connection) in knowledge gaps — may have undocumented dependencies.

## Related
- [[Features/Link Creation & XOR Obfuscation]]
- [[Database/secrets]]
- [[Architecture/Data Flows]]
