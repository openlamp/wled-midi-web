# wled-midi-web

**Drive a WLED device from any MIDI input, in the browser — no install, no server.**
A reference implementation of the [wled-midi](https://github.com/openlamp/wled-midi) convention
(`lamp` mode) as a single HTML file. Web MIDI → WLED's local JSON API.

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
  **`keymap`** (piano-aligned, LEDs-per-key), **`direct`** (note = LED index, sequencer). This is the
  **piano-guide / strip-instrument** path.

Commands are **coalesced into one `POST /json/state` per ~40 ms window** (per SPEC §7). One device
today; multi-channel routing and `mpe` are on the roadmap. Untested on hardware yet.

## License

[MIT](LICENSE) — implement it freely.

*Part of [OpenLamp](https://github.com/openlamp). Not affiliated with or endorsed by the WLED project;
it talks to WLED over its public local API.*
