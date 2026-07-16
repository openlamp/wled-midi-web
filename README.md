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

## Scope (v1)

Implements the **`lamp`** configuration of the unified syntax: notes 59–68 (looks, incl. black/white),
48–56 (util), 73 (flash), CC 1–8, Program Change → preset. Commands are **coalesced into one
`POST /json/state` per ~40 ms window** (per SPEC §7). One device (multi-channel routing, `strip`
and `mpe` modes are on the roadmap).

## License

[MIT](LICENSE) — implement it freely.

*Part of [OpenLamp](https://github.com/openlamp). Not affiliated with or endorsed by the WLED project;
it talks to WLED over its public local API.*
