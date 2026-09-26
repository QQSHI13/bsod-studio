# BSOD Studio

An interactive web app for designing your own **Blue Screen of Death** — any era, any language, animated — and saving it as a **PNG or GIF picture**.

Single self-contained `index.html` (all CSS/JS inlined, zero dependencies, works offline).

## Run it

```bash
xdg-open index.html            # or just double-click / open in any browser
python3 -m http.server 8080    # optional: http://localhost:8080
```

## Features

**Era presets** (layouts calibrated against real screenshots, incl. a genuine
1920×1080 Windows 10 capture):

| Preset | Year | Look |
|---|---|---|
| Windows NT 3.x | 1993 | `*** STOP:` + register/module dump, 80×50 text mode, `#0000A8` |
| Windows 95/98 | 1995 | "A fatal exception 0E…", 80×25 text mode, `#000080` |
| Windows 2000/XP | 2001 | "A problem has been detected…" + Technical Information |
| Windows 8/8.1 | 2012 | Sad face, inline "(0% complete)", cerulean `#1A67B3` |
| Windows 10 | 2016 | QR code, support info, "Your **PC** ran into a problem" |
| Windows 11 | 2021 | Same layout, "Your **device** ran into a problem" |
| Insider green | 2016 | GSOD `#246F24` |
| Black (2025) | 2025 | Newest design: black, centered, no QR/face, stop code + hex |

**Everything is editable:** background/text colors (+ authentic swatches), sad
face text, headline, progress %, link/QR target, support note, stop code
(datalist with 17 classics + live meaning), failed module, hex code in
parentheses, font, text size, alignment — and the raw 80-column text for
classic eras (NT dumps can be re-randomized with *Shuffle dump*).

**Output:** a **PNG ⇄ GIF format switch** drives the whole app — the download
button, stage badge, animation controls and preview all follow it.

- **GIF** — animation modes: percent count-up (0→100, adjustable frame delay
  30–400 ms, 4–40 steps, final frame held so the loop reads cleanly) and
  cursor blink for the classic text modes. Frame scrubber with play/pause
  under the preview to inspect any frame; total loop duration is shown.
  Shortcuts: `Space` play/pause, `Ctrl/⌘+S` download.
- **PNG** — still frame, controls hidden, zero clutter.

Sizes: 1080p, 1440p, 4K, 720p, 4:3, 800×600, 1080×1920 portrait, or custom.

**GIF encoder:** hand-written GIF89a encoder — variable-code-size LZW,
NETSCAPE2.0 looping, per-frame delays, global palette via exact-color
histogram (≤256 colors) with median-cut fallback, nearest-color mapping
cache. Verified by decoding output with Pillow: frame counts, per-frame
delays, exact flat colors, and quantization tolerance all assert-verified,
including streams crossing the 9→10→11→12-bit code boundaries.

**Colors:** 21 background swatches (all six authentic eras + Windows-accent
extras) and 8 text swatches, plus free hex/color pickers.

**i18n:** the UI *and* the on-screen BSOD text are fully translated —
English, 简体中文, 日本語, Español, Français, Deutsch. Switching language
regenerates non-edited fields with authentic localized templates (e.g.
„Auf dem PC ist ein Problem aufgetreten…"). Edited fields are never
overwritten (per-field dirty tracking). Settings persist in `localStorage`.

**History tab:** an illustrated timeline of the BSOD 1985→2025 (mojibake boot
screens, NT 3.1, COMDEX 1998, `c:\con\con`, Ballmer's prose, the 2016 QR,
CrowdStrike's 8.5M-device outage, the 2025 black redesign) with "load in
editor" buttons for the legendary screens.

## QR codes

`assets/js/qrcode.js` is a from-scratch encoder: byte mode, EC level **M**,
versions 1–40, all 8 masks with ISO 18004 penalty scoring, anisotropic
text-mode scaling not required 😄 — ~330 lines, zero dependencies.

**Verified**, not just written: the test harness renders the generated
matrices to bitmaps and **decodes them with ZXing C++** across v1–v39,
ASCII/CJK/emoji payloads up to 2200 bytes — all decode byte-perfect.
(Two bugs were found and fixed this way: an alignment-pattern placement rule
that wrongly skipped patterns overlapping the *timing* row on v≥7, and a
font-stack fallback that broke classic-era monospace rendering.)

## Project layout

```
index.html            everything — app shell, styles, all scripts inlined
README.md
```

Previously split into `assets/js/{i18n,presets,qrcode,gif,render,app}.js`
(+ CSS); the inline order in `index.html` is the same.

## Fidelity notes

- Modern layout constants were measured from a real Windows 10 2004 capture
  (1920×1080): face top 0.171H, headline pitch 0.0546H, percent =
  headlineTop + (lines+0.5)·pitch, QR = 0.109H at percentTop + 1.5·pitch,
  margin 0.19H — the flow model reproduces every measured band within a
  pixel or two.
- Windows 8 content indent (0.294H) and the centered 2025 layout
  (headline 0.465H, stop line 0.946H) measured from official-era captures.
- Classic eras are true 80-column text grids (NT 50 rows, 9x 25, XP 30) with
  anisotropic glyph scaling like real text mode.
- On non-Windows systems "Segoe UI" falls back to the system font; on
  Windows the preview is pixel-authentic.

## Sources for the history tab

Wikipedia — *Blue screen of death* · CNBC (Jun 26, 2025: black BSOD announcement) ·
Microsoft Windows Insider Blog (Mar 28, 2025: new layout default) ·
The Verge · Microsoft Learn · CrowdStrike incident reports (Jul 19, 2024).

*Parody/picture generator. Not affiliated with Microsoft; Windows is a
Microsoft trademark.*
