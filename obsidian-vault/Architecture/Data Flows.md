# Data Flows

## 1. Creation Flow
```
POST /create
  → generate random public_token + control_token
  → XOR-obfuscate content with public_token as key
  → INSERT into secrets table
  → return { secretUrl, controlUrl }
```
Participants: [[Modules/routes/create.js]], [[Features/Link Creation & XOR Obfuscation]], [[Database/secrets]]

---

## 2. Visit Flow
```
GET /s/:token
  → logger middleware fires FIRST
    → extractIP() → lookupIP() → logAccess()
    → INSERT into access_logs
  → render honeypot template (Nunjucks)
```
Participants: [[Modules/logger.js]], [[Modules/geo.js]], [[Database/access_logs]]

---

## 3. Reveal Flow
```
POST /s/:token/reveal
  → XOR-decode content (public_token is key)
  → update access_logs: reveal_attempted=1, reveal_succeeded=1
  → if burn_on_reveal=true → mark secret burned
  → fire webhook (first_access / reveal events)
  → return decoded content
```
Participants: [[Modules/routes/secret.js]], [[Features/Burn on Reveal]], [[Modules/webhook.js]]

---

## 4. GPS Flow
```
GET /s/:token  (if require_location=true)
  → server renders page with location prompt flag
  → public/secret.js prompts browser geolocation API
  → if denied → generate QR code for mobile scan
  → on grant → beacon POST → INSERT into location_beacons
```
Participants: [[Features/GPS Tracking]], [[Database/location_beacons]], [[Features/QR Code Generation]]

---

## Graph Hyperedges (extracted at confidence 1.0)
- Creation: token generation → XOR obfuscation → SQLite storage
- Visit: logger middleware → geolocation → access_logs
- GPS: require_location → browser prompt → location_beacons
- Viz: Chart.js + world-map.svg + us-map.svg → control panel
