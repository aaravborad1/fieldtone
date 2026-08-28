# FieldTone

A free, offline, installable **metronome + tuner + tone generator + recorder** for marching band —
a TonalEnergy-style app that runs in iPhone Safari with nothing to buy.

Single file: `index.html` (no build, no dependencies). Everything is synthesized or computed in-browser.

## Features

### Metronome
- Sample-accurate Web Audio scheduler (lookahead), 10–400 BPM, tap tempo, ±1 / ±10
- Any time signature; "count as" beat unit incl. dotted-quarter for 6/8, 9/8, 12/8
- Subdivisions: eighths, triplets, sixteenths, sextuplets, swing (adjustable %), dotted 8th–16th
- Per-beat accent editor: normal / accent / soft / silent
- 9 synthesized click voices (wood block, click, clave, cowbell, digital, hi-hat, rimshot, membrane, beep), separate voice for subdivisions, pitch trim
- Count-in bars, full-screen flash for the drum major, vibrate on downbeat, screen wake-lock
- Tempo trainer (auto-increase), random bar-mute practice mode
- Optional spoken beat numbers

### Show builder — the tempo map for the whole show
- A **Show** = ordered list of **segments**; each segment is a run of measures at one time signature + tempo
- Per-segment **tempo ramp** (accel. / rit.), subdivision, accent-1 toggle, rehearsal label
- Measure numbers computed automatically; total measures + duration shown
- Playback follows the whole show: live measure counter, current-segment highlight, "next: letter @ tempo"
- **Start from any measure**
- Save shows to an on-device library; **Export / Import `.json`** files to share with other directors
- **Render click track to `.wav`** — the entire show (with count-in) as one audio file to AirDrop and run through the field PA, no app needed on the field

### Tuner
- Autocorrelation pitch detection (parabolic interpolation), ~Bb1–C6
- The smiley: grins green within ±5 cents, turns amber then red as you drift
- Needle or strobe display, cents-history graph
- A4 calibration 410–470 Hz, transposition presets (B♭, E♭, F, D, G)
- Input-device picker (choose an interface / Bluetooth mic to tune off the field mics)

### Tone / drone
- Sustained reference pitch, MIDI 24–108, sine / triangle / saw / square / stacked organ
- 12-button pitch pipe; runs alongside the metronome and tuner

### Recorder
- Records to `.m4a` (iOS) / `.webm`, waveform preview, playback, download
- Option to run the click while recording

### Audio output / Bluetooth
- On iPhone the click follows whatever output iOS is using — pair the field PA in **Settings → Bluetooth**, or cable the phone into the board. Nothing to configure.
- On desktop browsers that support it, an explicit output-device selector is shown.

## Run locally

```bash
node server.mjs        # serves this folder on http://localhost:8123
```

Mic features (tuner, recorder) need HTTPS or `localhost`.

## Put it on an iPhone

The tuner/recorder need HTTPS. Host the folder anywhere static and free:

- **GitHub Pages** — push this folder to a repo, enable Pages, open the URL in iPhone Safari
- **Netlify Drop / Cloudflare Pages** — drag the folder in
- Then in Safari: **Share → Add to Home Screen**. It installs as a standalone app and works offline.

## Files

| file | purpose |
|---|---|
| `index.html` | the entire app |
| `manifest.webmanifest` | PWA install metadata |
| `sw.js` | service worker (offline cache, network-first for the app shell) |
| `icon.svg` | app icon (a canvas-drawn PNG apple-touch-icon is injected at runtime for iOS) |
| `server.mjs` | local static server for testing |

## QA performed

- Time-signature / beat-unit math verified for 4/4, 3/4, 6/8 (eighth + dotted-quarter), 2/2, 5/4, 12/8
- Metronome scheduler runs clean, advances correctly, recovers from voice errors without stopping
- Multi-segment show playback: segment transitions, meter changes, tempo ramps, start-from-measure, completion
- Offline `.wav` render: valid RIFF/WAVE header, count-in prepended, no clipping
- Pitch detection accuracy within ~1–2 cents 220–1047 Hz (±5 at the extreme low end); no false detection on silence
- Transposition (concert / B♭ / E♭ / F) note mapping
- Drone start / live pitch change / clean stop
- Graceful handling when mic permission is denied
- PWA: service worker registers & activates, manifest + icons resolve, apple-touch-icon injected
- Verified on mobile (375×812) and desktop viewports; console clean throughout
