# Lichtbrücke

Browser app that transfers files between two devices via rotating QR codes. No server, no uploads. Single self-contained `index.html`, libraries from CDN (qrcodejs for encoding, jsQR for decoding).

## Run
Camera access (`getUserMedia`) needs HTTPS or localhost:
`python3 -m http.server 8000` → http://localhost:8000

## Protocol
- File split into sequential blocks, each shown as one QR code, looping forever.
- Block = JSON `{id, i, n, name, size, type, h, d}` (transfer id, index, total, filename, size, MIME, SHA-256 of whole file, base64 data).
- Receiver collects blocks in any order across cycles, assembles when complete, verifies SHA-256.

## Current features
- Sender: block size (numeric), ECC level L/M/Q/H, normal/inverted colors, fps, live stats (size, blocks, cycle time, throughput, elapsed, cycles).
- Receiver: camera picker, torch toggle (if supported), scan resolution, inversion mode, live stats (rate, ETA, throughput, frames, duplicates).

## Tuning notes
- ECC level L is enough; SHA-256 covers integrity.
- ~600 bytes/block is the sweet spot for scan reliability.
- Camera practically caps at ~10–15 fps.
- Throughput lever with most upside: multiple QR codes per frame (e.g. 2×2 grid) rather than bigger blocks.
- Data Matrix (bwip-js + ZXing) was tried and dropped. QR only.

## Next
- 2×2 multi-QR grid per frame.
- Static HTTPS hosting (Cloudflare Pages / Netlify Drop / GitHub Pages).

## Working style
Terse, decisive. Deliver complete working changes, don't over-explain.
