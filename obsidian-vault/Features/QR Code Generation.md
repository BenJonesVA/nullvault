# QR Code Generation

Generates a QR code SVG when desktop browser denies location access.

## Purpose
Fallback for [[Features/GPS Tracking]] — if `require_location=true` but browser denies geolocation (common on desktop), serve a QR code the target can scan with their phone to grant mobile GPS access.

## Implementation
- `src/qr.js` — uses `qr.js` library
- `toSVG()` — renders QR as inline SVG (no external image CDN needed)

## Module
[[Modules/qr.js]]

## Graph Community
Community 24 "QR Code Generation" — thin community (2 nodes: `toSVG()`, `qr.js`).
