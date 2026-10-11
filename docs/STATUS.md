# Status

**Updated:** 2026-10-10 · **Current phase:** 3 (Breadboard prototype and firmware), hardware testing
**Versions:** firmware 0.1.0 (untagged) · protocol 0.1.0 · app: not started

## Where we left off
- **Doing:** PLAN 3.1, wiring the breadboard, following `~/dial-firmware/BREADBOARD.md`
  (picture: `~/dial-firmware/docs/breadboard_map.png`).
- **Last done:** Step 2. The XIAO is pressed into the 400-point breadboard, rows 1-7 across the
  groove, USB toward row 1. USB cable unplugged. No wires yet.
- **Next:** Step 3, the ground wires: wire 1 from j2 to the right blue line (the outer line on
  the right), wire 2 bridging the two blue lines just below row 30 (the left blue line is the inner
  one, next to column a). Then Steps 4-6, then a top-down photo for Claude to check **before**
  USB goes in, then fw README B3 (upload the firmware, Serial Monitor).

## Done
- Phase 0: tools, accounts, repos, test phone (iPhone 18 Pro, iOS 27).
- Phase 1: `SPEC.md` and `V2.md` in `dial-app` (copy of `SPEC.md` in `dial-firmware`).
- Phase 2: kit and tools arrived 2026-10-09; blink, USB drive and encoder checks passed 2026-10-10
  (polarity check still waits on the pigtail).
- Phase 3: `PROTOCOL.md` 0.1.0 (both repos); firmware 0.1.0 compiles at 125 KB (15%); fw README;
  `BREADBOARD.md` layout; XIAO placed on the breadboard.

## Blocked
- Battery polarity check (Phase 2) and battery soldering (3.5): JST-PH pigtail not confirmed yet.

## Decisions needed
1. **ISO (D9) and WB (D10) buttons.** `SPEC.md` has them; firmware and `PROTOCOL.md` 0.1.0 don't.
   Recommended: test 0.1.0 on the breadboard first, then add them as 0.2.0 (two more switches,
   nothing rewired).

## Next 3 actions
1. Finish 3.1: `BREADBOARD.md` Steps 3-6, photo check.
2. fw README B3: upload the firmware, check the Serial Monitor (boot lines, `[dial]` counts,
   button events). Report the output.
3. fw README B4: nRF Connect test on the iPhone, 20 s screen capture.

## After that
`DEBUG_WAIT_FOR_SERIAL_MS` is 3000 for breadboard testing (set back to 0 before battery tests) →
Phase 3 Done-when checklist (fw README) → pigtail arrives: polarity + Part C battery →
ISO/WB as 0.2.0 → tag. Phase 0 leftovers (BLE explainer, AVCam) still open.
