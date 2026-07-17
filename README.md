# wled-midi-web

**Drive a WLED device from any MIDI input, in the browser — no install, no server.**
A reference implementation of the [wled-midi](https://github.com/openlamp/wled-midi) convention
(`lamp` · `strip` · `mpe` modes) as a single HTML file. Web MIDI → WLED's local JSON API.

## Use

1. Open `index.html` in **Chrome or Edge** (Web MIDI isn't in Firefox/Safari).
2. Enter your **WLED device IP**, click **Enable MIDI**, pick your MIDI input, and play.
3. Notes → colour looks / power, CC 1–8 → brightness/hue/sat/CCT/effect, Program Change → preset.

### Zero-install deployment (recommended)

Upload `index.html` to the **WLED device filesystem** (WLED web UI → *File Editor*) and open
`http://<wled-ip>/index.html`. Served from the device, it's **same-origin** with the JSON API —
no CORS.

> ⚠ **Web MIDI needs a secure context.** A plain `http://<ip>` page blocks MIDI. Use `localhost`,
> `https`, or a reverse proxy (see the [wled-midi tooling docs](https://github.com/openlamp/wled-midi#credits--prior-art)).
> Running the file locally instead? The browser will block cross-origin requests to WLED — serve
> it from the device, or use a proxy.

## Modes

- **`lamp`** (control) — notes 59–68 (looks, incl. black/white), 48–56 (util), 73 (flash), CC 1–8
  (bri/cct/hue/sat/fx/sx/ix/pal), Program Change → preset.
- **`strip`** (position) — note → LED position, **polyphonic**, velocity → brightness, individual-LED
  payload. Three position functions: **`interpolate`** (note range → strip, melodic/stage),
  **`keymap`** (piano-aligned, LEDs-per-key), **`direct`** (note = LED index, sequencer). Note-off
  **fade** (configurable), and the note's **MIDI channel picks the colour** (Synthesia L/R hands).
  This is the **piano-guide / strip-instrument** path.
- **`mpe`** (expressive voice-zones) — MIDI channel = a per-note voice placed as a **zone of LEDs at the
  pitch position** on the strip (SPEC §13), **polyphonic**: pitch → position + base hue, channel pressure →
  brightness, CC74 slide → saturation, pitch-bend → hue shift. Uses the same strip config as `strip`.

Commands are **coalesced into one `POST /json/state` per ~40 ms window** (per SPEC §7). One device
today (multi-channel routing on the roadmap). Untested on hardware yet.

## License

[MIT](LICENSE) — implement it freely.

*Part of [OpenLamp](https://github.com/openlamp). Not affiliated with or endorsed by the WLED project;
it talks to WLED over its public local API.*

---

**Two open standards, one bridge.** This implements the open [**wled-midi**](https://github.com/openlamp/wled-midi) convention — the agreed dictionary between [**MIDI**](https://midi.org) (the MIDI Association) and [**WLED**](https://kno.wled.ge). Free for anyone to build on: see the convention's [openness & patent policy](https://github.com/openlamp/wled-midi/blob/main/SPEC.md) (§14) and the [licensing note](https://github.com/openlamp/wled-midi/blob/main/docs/licensing.md). Part of [OpenLamp](https://github.com/openlamp).
