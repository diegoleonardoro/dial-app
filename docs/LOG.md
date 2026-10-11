# Log

Append-only. One entry per session: date, repo(s), what changed, commits.

---

### 2026-10-02 · both repos
- Created `dial-firmware` and `dial-app` on GitHub (Phase 0).
- Wrote `SPEC.md` and `V2.md` in `dial-app` (Phase 1); copied `SPEC.md` into `dial-firmware`.
- Commits: app `badce00`, `bfcdd81`; fw `a5fced3`, `1c9987d`.

### by 2026-10-05 · purchasing
- Breadboard kit ordered (Phase 2). Not yet arrived.

### 2026-10-05 · dial-firmware, dial-app
- Phase 3 no-hardware work: `PROTOCOL.md` 0.1.0, firmware 0.1.0 (encoders, buttons, ble_link,
  battery, status_led, power), fw `CLAUDE.md`, fw README Parts A-C and the Phase 3 checklist.
  Compiles at 125 KB (15%).
- Commits: fw `7cfdce2`; app `46ccc47` (`PROTOCOL.md` copy).

### 2026-10-05 · dial-app, dial-firmware (tracking setup)
- Added `docs/PLAN.md`, `STATUS.md`, `DECISIONS.md`, `LOG.md` and `CLAUDE.md` in `dial-app`.
- Added a tracking section to fw `CLAUDE.md`; `/status`, `/checkpoint`, `/gaps` commands in both repos.
- Re-verified the firmware compile (125,468 bytes).
- Found: ISO/WB buttons are in `SPEC.md` but not in firmware or `PROTOCOL.md` (PLAN 3.6).
- Diego confirmed: tools and JST-PH pigtail ordered (none arrived yet); AVCam sample and BLE
  explainer not done yet (left open in PLAN Phase 0).

### 2026-10-09 · purchasing
- Diego confirmed the tools (iron, multimeter, etc.) have arrived. Breadboard kit and JST-PH pigtail not confirmed yet.
- Diego confirmed the breadboard kit has arrived. Phase 2 arrival item ticked; blink, encoder,
  wiring, upload and nRF Connect steps unblocked. Polarity check and battery soldering wait on the
  JST-PH pigtail.
- fw `dial_config.h`: `DEBUG_WAIT_FOR_SERIAL_MS` 0 -> 3000 so the boot lines show in the Serial Monitor
  during breadboard testing (set back to 0 before battery tests).

### 2026-10-10 · hardware (Phase 2 checks)
- Diego: blink sketch uploaded ("Device programmed", port /dev/cu.usbmodem101), double-tap reset
  shows the USB drive, encoders click cleanly. Phase 2 Done-when: 3 of 4 ticked (polarity waits on
  the pigtail). Next: wire the breadboard (B2).
- Part 2 (wiring) started on the 400-point breadboard. Layout saved as fw `BREADBOARD.md` and
  `docs/breadboard_map.png` (left blue line is the inner one on this board). Steps 1-2 done: board
  positioned, XIAO pressed in at rows 1-7. Next: Step 3, ground wires.
- Added `/resume` command and a "Where we left off" section in `STATUS.md`.
- Commits: fw `a0a825c` (BREADBOARD.md, map, /resume, serial wait); app: tracking files, CLAUDE.md,
  commands (this commit).
