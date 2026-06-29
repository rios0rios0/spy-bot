# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Spy Bot is a **discontinued** (September 2018) Arduino surveillance robot. The entire firmware is one sketch, `main.ino`. It runs an embedded HTTP server that serves an inline HTML control panel; the page drives four GPIO motor pins and embeds an MJPEG stream from a networked IP camera. No new features or fixes are planned — preserve historical behavior when editing.

## Build / test / lint

There is no build system, test suite, or linter — this is embedded firmware. Open `main.ino` in the Arduino IDE and upload over USB. `.github/workflows/release.yaml` only tags releases on pushes to `main`; it does not compile the sketch.

## Non-obvious architecture

- **HTTP parsing is character-by-character, not line-based.** `loop()` reads one char at a time, appends to a `String` once a `G` is seen, and every 10th char (`i == 10`) scans the accumulated buffer for `?go=E/D/F/T/O`, then resets the buffer. There is no real request parser — adding routes means extending this windowed scan (`main.ino:50-65`).
- **Motor control is differential drive over an H-bridge** on pins 4–7. Each command sets a specific pin pair HIGH (`main.ino:67-92`): Forward 7+5, Backward 4+6, Left 7+4, Right 6+5, Off all LOW. Turning runs the two motors in opposition.
- **Network config is hardcoded**, no DHCP: MAC at `main.ino:8`, static IP `192.168.1.101` at `main.ino:12`, camera `192.168.1.200:81` embedded in the served HTML (CGI links and the `videostream.cgi` iframe). Camera pan/tilt links target a hidden `null` iframe so the page does not reload.
- **The whole UI is generated inline** via `client.println()` calls in the `\n`-on-blank-line block. There are no separate templates served at runtime; the `files/` directory holds standalone prototypes (`index.html`, `testehtml.html`, `hold.js`) that the sketch does **not** serve.

## Conventions

- Keep network constants hardcoded; this is the established pattern, not a bug.
- Generate HTML with `client.println()` rather than building large strings.

## Further reference

`README.md` documents hardware requirements and wiring. `.github/copilot-instructions.md` covers the same ground for GitHub Copilot in more detail (pin tables, common edit locations) — keep the two consistent when one changes.
