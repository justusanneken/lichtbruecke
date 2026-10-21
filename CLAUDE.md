# Lichtbrücke

Browser app that transfers files between two devices via rotating QR codes. No server, no uploads. Single self-contained `index.html`, libraries from CDN (qrcodejs for encoding, jsQR for decoding).

## Run
Camera access (`getUserMedia`) needs HTTPS or localhost:
`python3 -m http.server 8000` → http://localhost:8000

## Protocol
- File split into sequential blocks, each shown as one QR code, looping forever.
- Block = JSON `{id, i, n, name, size, type, h, d[, g]}` (transfer id, index, total, filename, size, MIME, SHA-256 of whole file, base64 data, optional grid side length).
- Receiver collects blocks in any order across cycles, assembles when complete, verifies SHA-256.

## Current features
- Sender: codes per frame (1 / 2×2 / 3×3), block size (numeric), ECC level L/M/Q/H, normal/inverted colors, fps, live stats (size, blocks, cycle time, throughput, elapsed, cycles).
- Receiver: auto-detects grid from `g`, decodes several codes per frame (jsQR reads one code per call, so the frame is split into g×g tiles with 5% overlap; the grid must fill the viewfinder; jsQR fails when several codes share one image, masking didn't help); camera picker, torch toggle (if supported), scan resolution, inversion mode, live stats (rate, ETA, throughput, frames, duplicates).

## Tuning notes
- ECC level L is enough; SHA-256 covers integrity.
- ~600 bytes/block is the sweet spot for scan reliability.
- Camera practically caps at ~10–15 fps.
- Throughput lever with most upside: multiple QR codes per frame (2×2 grid) rather than bigger blocks. With 3×3 use smaller blocks (~300 B).
- Data Matrix (bwip-js + ZXing) was tried and dropped. QR only.

## Hosting
Vercel, static, no build: `vercel.json` sets output dir `.`, camera Permissions-Policy and basic headers. Deploy: import the repo in Vercel (framework preset: Other) or run `vercel --prod`. HTTPS is automatic, so `getUserMedia` works.

## Next
- Test grid mode on real devices; tune default block size per grid.

## Working style
Terse, decisive. Deliver complete working changes, don't over-explain.
