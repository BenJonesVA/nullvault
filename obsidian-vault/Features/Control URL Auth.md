# Control URL Auth

NullVault has no login system. The `controlUrl` token IS the password.

## How It Works
- `control_token` generated at creation time
- Control panel URL: `BASE_URL/s/<public_token>/control?t=<control_token>`
- Server compares `?t=` param against `secrets.control_token`
- No session, no cookie, no login form

## Implications
- Lose the URL = lose access to the control panel
- URL leakage = full dashboard access
- Intentionally simple — this is a personal research tool

## Graph
- Community 34 "Control URL Auth" — node: `controlUrl as Password (No Separate Auth System)`
- Community 51 "Control URL Feature"
