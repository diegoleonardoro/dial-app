# Dial Controller: Master Plan

Source: *iPhone Camera Dial Controller: Zero-to-Prototype Guide* (Oct 1, 2026), 10 phases (0 to 9).
This file is the checklist. `STATUS.md` is the short snapshot, `DECISIONS.md` says why,
`LOG.md` says when.

**Legend:** `- [x]` done (with evidence) · `- [ ]` not done · tags after a dash:
**IN PROGRESS**, **BLOCKED: reason**, **UNCONFIRMED** (Diego to confirm), **ADDED** (not in the guide).

**Rule:** a box is ticked only when the evidence exists (a file, a commit, a recording, a
measured number). Each phase's "Done when" list is the gate to the next phase.

**Repos:** `dial-firmware` (fw) and `dial-app` (app), both on GitHub under `diegoleonardoro`.
Tracking files live only in `dial-app/docs/`.

---

## Phase 0: Prerequisites

### Done when
- [x] Xcode 27 installed; Hello World app run on own iPhone over USB (Diego, 2026-10-05)
- [x] Arduino IDE 2.x installed (fw compiles with it)
- [x] nRF Connect installed on the iPhone (Diego, 2026-10-05)
- [x] Two GitHub repos exist: `dial-firmware`, `dial-app`
- [x] First test iPhone decided and written down: iPhone 18 Pro on iOS 27 (`SPEC.md`, Target)

### Skill prep (guide: "before Phase 3")
- [x] Flash a blink sketch from the Arduino IDE (Diego, 2026-10-10: "Device programmed" on /dev/cu.usbmodem101, LED blinking)
- [ ] Read a one-page BLE explainer (peripheral, central, service, characteristic, notify, pairing). Not done; do while waiting for parts
- [ ] Run Apple's AVCam sample on the iPhone and take one photo. Not done; do while waiting for parts
- [x] Claude Code set up in a terminal, pointed at a repo

## Phase 1: Define the product

### Done when
- [x] One-page `SPEC.md` in `dial-app` with the controls table filled in (2026-10-02, copy in fw)
- [x] Spec names the first test iPhone and what the front dial does on it (18 Pro, aperture)
- [x] `V2.md` holds everything cut
- [x] One sentence per control ("Each control in one sentence" in `SPEC.md`)

## Phase 2: Buy the parts

- [x] Breadboard kit ordered (XIAO x2, encoders, tactile switches, two-stage shutter, LiPo, slide switches, knobs, breadboard/wires/USB-C). Arrived (Diego, 2026-10-09)
- [x] Tools ordered (soldering iron, multimeter, flux, cutters, tweezers, heat-shrink, glue gun) (Diego, 2026-10-05). Arrived (Diego, 2026-10-09)
- [x] JST-PH 2.0 two-pin socket pigtail ordered. **ADDED** (fw README Part C; not in the guide's list) (Diego, 2026-10-05). Arrival not confirmed
- [ ] Final-build parts (phone clamp, M2/M2.5 screws + heat-set inserts, 26 AWG silicone wire): order after Phase 3 works

### Done when (fw README Part B1 / C1)
- [x] All breadboard-kit parts are on the desk (Diego, 2026-10-09: kit and tools arrived; JST-PH pigtail not confirmed yet)
- [x] XIAO shows up as a USB drive when double-tapped and runs a blink sketch (Diego, 2026-10-10)
- [ ] Battery connector polarity confirmed with a multimeter and written on tape on the battery. **BLOCKED: JST-PH pigtail** (battery and multimeter are here; fw README C1)
- [x] Each encoder clicks cleanly through 24 detents, no dead spots (Diego, 2026-10-10)

## Phase 3: Breadboard prototype and firmware

- [ ] 3.1 Wire the breadboard per the pin table (fw README Part B2). **IN PROGRESS**. Hole-by-hole steps in fw `BREADBOARD.md`, map in fw `docs/breadboard_map.png`
  - [x] Step 1: 400-point breadboard positioned, row 1 at top, column a on the left (Diego, 2026-10-10)
  - [x] Step 2: XIAO pressed in, rows 1-7 across the groove, USB toward row 1 (Diego, 2026-10-10)
  - [ ] Step 3: ground wires 1-2 (j2 to right blue line; bridge between the blue lines below row 30)
  - [ ] Step 4: encoders and wires 3-12
  - [ ] Step 5: HALF / FULL / MODE switches and wires 13-18
  - [ ] Step 6: top-down photo checked by Claude before plugging in USB
- [x] 3.2 BLE protocol designed: UUIDs generated, `PROTOCOL.md` 0.1.0 identical in both repos (2026-10-05)
- [x] 3.3 Firmware written as separate modules: encoders, buttons, ble_link, battery, status_led, power (fw `7cfdce2`)
  - [x] Compiles on Seeed nRF52 Boards 1.1.13 + Bounce2: 125,468 bytes (15%), re-verified 2026-10-05
  - [x] fw README written with Part A (no hardware), Part B (wiring, upload, nRF Connect), Part C (battery), troubleshooting
- [ ] 3.3b Upload to the board, Serial Monitor boot lines and dial counts correct (fw README B3). Next, after 3.1
- [ ] 3.4 nRF Connect test: `01 01 00 nn` / `01 FF 00 nn+1`, Feedback write `0102` turns LED green; 20 s screen capture saved (fw README B4). Next, after 3.3b
- [ ] 3.5 Battery soldered via slide switch; `[bat]` voltage within 0.05 V of the multimeter (fw README Part C). **BLOCKED: JST-PH pigtail**
- [ ] 3.6 ISO (D9) and WB (D10) buttons in firmware, protocol and wiring table. **ADDED.** `SPEC.md` defines them (raw press/release from the firmware), but `PROTOCOL.md` 0.1.0, `dial_config.h`, `buttons.cpp` and the README wiring table do not have them yet. Needs new event codes in both `PROTOCOL.md` copies. See STATUS "Decisions needed"

### Done when (fw README "Done-when checklist")
- [ ] Every control produces the correct event in nRF Connect, including half vs full shutter
- [ ] Ten fast full rotations of each dial = 240 ± 2 total delta
- [ ] Sequence numbers never skip during 2 minutes of random input
- [ ] Sleeps after 10 min idle, wakes on a button press; LED states correct
- [ ] Stays connected 8 hours on the battery (nRF Connect overnight)
- [x] `PROTOCOL.md` and firmware committed; firmware version string is `0.1.0`
- [ ] Tag `fw-0.1.0` pushed (only after every box above is ticked)

## Phase 4: The iPhone app

- [ ] 4.1 Xcode 27 project in `dial-app`: SwiftUI, iOS 27 minimum, Swift 6 strict concurrency, started from AVCam; Personal Team signing
- [ ] 4.1b Info.plist: `NSCameraUsageDescription`, `NSMicrophoneUsageDescription`, `NSPhotoLibraryAddUsageDescription`, `NSAccessorySetupKitSupports` = [Bluetooth], `NSAccessorySetupBluetoothServices` = [`188D0001-8E0D-4A9D-9B04-830DCBEB93B8`]
- [ ] 4.2 `CameraService` (session, back camera, photo + movie outputs, preview; from AVCam)
- [ ] 4.2 `ExposureModel` (pure Swift: value lists from the active format, index stepping, modes M/A/S/Auto, fixed-aperture rules from `SPEC.md`)
- [ ] 4.2 `ControllerClient` (AccessorySetupKit pairing, Core Bluetooth subscribe, parse 4-byte events, sequence-gap logging, AsyncStream)
- [ ] 4.2 `CaptureView` (preview + HUD: f/, shutter, ISO, mode, battery, connection state)
- [ ] 4.3 Exposure calls on the session queue inside `lockForConfiguration`, validated with `supportsExposureModeCustom`; `photoQualityPrioritization = .speed`; at most one exposure change per preview frame
- [ ] 4.4 Pairing flow: `ASAccessorySession` picker with name + image; auto-reconnect on foreground and controller wake
- [ ] Swift constants for the UUIDs and event codes diffed against `PROTOCOL.md`
- Build order (run on the phone after each):
  - [ ] Preview + on-screen shutter button
  - [ ] Manual exposure from on-screen buttons
  - [ ] Controller connection, events logged to the console
  - [ ] Controller driving exposure
  - [ ] Priority modes (A, S, Auto) and ISO/WB hold buttons
  - [ ] Video
  - [ ] Polish

### Done when
- [ ] Rear dial changes shutter speed in the HUD and the preview, one third stop per detent
- [ ] Front dial changes aperture on the 18 Pro (behavior on fixed-aperture cameras per `SPEC.md`), HUD says which
- [ ] Photo at 1/1000 s shows 1/1000 s in Photos EXIF; same for ISO and f-number
- [ ] Half press locks focus; full press captures; rear dial press cycles lenses
- [ ] Dial-to-preview latency under 100 ms (film at 60 fps, count frames)
- [ ] AccessorySetupKit picker once, then reconnects on its own after sleep, power cycle, app relaunch
- [ ] `ExposureModel` unit tests pass; app tagged `app-0.1.0`

## Phase 5: Integration and field testing

- [ ] 5.1 Lab mule: breadboard + battery + knobs on foam board, strapped to a phone case
- [ ] 5.1 Three sessions: bright day outdoors, indoors in the evening, one video session
- [ ] 5.2 Measure (write the number next to each):
  - [ ] Dial-to-preview latency (target < 6 frames at 60 fps): ___
  - [ ] Lost detents, 10 fast turns (target within 2 of 240): ___
  - [ ] Reconnect time after sleep (target < 3 s): ___
  - [ ] Battery life connected, 500 mAh (target > 8 h): ___
  - [ ] Range, line of sight (target > 5 m): ___
  - [ ] Exposure accuracy, gray card at 3 shutter speeds (target exact): ___
  - [ ] Interference next to Wi-Fi laptop + BT headphones (target no dropouts in 10 min): ___
- [ ] 5.3 Feel decisions written into `SPEC.md` and implemented (currently provisional: Nikon direction, thirds, no acceleration, no wrap)
  - [ ] Rear dial clockwise = faster or slower shutter
  - [ ] Front dial: third stops or the 4 recommended stops on the 18 Pro
  - [ ] Fast spin: accelerate or one-to-one
  - [ ] Half press locks exposure in Manual or only in priority modes
- [ ] 5.4 (optional) Apple Developer Program ($99), archive, TestFlight internal testers

### Done when
- [ ] Every test row has a measured number
- [ ] Three sessions done; at least 5 keeper photos
- [ ] The four feel decisions are in `SPEC.md` and implemented
- [ ] Firmware and app tagged `0.2.0`

## Phase 6: Enclosure and physical dials

- [ ] 6.1 Choose print route (service for v1-v2; own printer from v3 on)
- [ ] 6.2 Calipers bought; XIAO and encoder STEP files downloaded; parts measured
- [ ] 6.2 CAD: OpenSCAD internal frame (all dimensions as named variables) + Onshape/Fusion outer shell
- [ ] 6.3 Command dial wheels: ~20 mm, 3-4 mm thick, D-bore 0.1 mm under, 36-48 ridges, resin if possible; cardboard mock-ups first
- [ ] 6.3b Fit the ISO and WB buttons; check whether the grip needs ~10 mm more height (`SPEC.md` Form factor)
- [ ] 6.4 Design rules: 2 mm walls, 0.3 mm clearance (0.5 mm battery), M2 screws + heat-set inserts, antenna end against an outer wall, USB-C / switch / light-pipe cut-outs, PETG or ASA
- [ ] 6.5 Phone attachment: spring clamp on a printed 1/4-20 stud
- [ ] 6.6 Print v1 (empty fit check), v2 (electronics in), v3 (after a week of shooting); STL + photo of each committed

### Done when
- [ ] Every component inside the shell, held mechanically, lid screwed shut
- [ ] Both dials click crisply, no wobble or rubbing
- [ ] Charges and flashes through the USB-C cut-out without opening
- [ ] Survives a desk-height drop onto a wood floor
- [ ] Range unchanged with the shell closed and a hand on the grip
- [ ] 30 minutes one-handed shooting without the grip feeling wrong

## Phase 7: Custom PCB (optional; only after a month with the Phase 6 unit)

- [ ] KiCad 9 installed; tutorials done
- [ ] Carrier PCB ~35 x 70 mm: XIAO module, 2 encoders, ALPS shutter, mode + ISO + WB switches, JST-PH, slide switch, RGB LED
- [ ] Order from JLCPCB or PCBWay

### Done when
- [ ] Board arrives; all parts fit the footprints first time (1:1 paper print check)
- [ ] Same firmware runs with no pin changes beyond the pin table
- [ ] Phase 6 enclosure revised to hold the board on standoffs
- [ ] Schematic, layout and BOM with vendor part numbers committed

## Phase 8: Apple

- [x] Firmware connection parameters follow Apple's Accessory Design Guidelines (15-30 ms, see `PROTOCOL.md`)
- [ ] Re-check advertising and reconnection behavior against the Accessory Design Guidelines PDF
- [ ] No `external-accessory` in `UIBackgroundModes`; no background mode at all
- [ ] Naming follows Apple trademark rules ("works with iPhone"; no "iPhone" in the product name)
- [ ] On-screen manual controls, so the app works without the controller
- [ ] "Demo controller" toggle that simulates dial events
- [ ] Privacy manifest + camera / microphone / photo-library purpose strings
- [ ] 60-second video of the real controller driving the app

### Done when
- [ ] A TestFlight build installed by someone else, from a link, on their own phone
- [ ] App works with and without the controller
- [ ] Review notes, demo video and demo mode exist
- [ ] (If App Store) app approved and live

## Phase 9: Selling it (only if you want to)

### Done when
- [ ] Quote from one FCC-accredited lab; asked the modular-approval question about the module
- [ ] Decision on Bluetooth SIG fee vs pre-selling, written down with the date (`DECISIONS.md`)
- [ ] Per-unit cost sheet for the intended volume
- [ ] Battery supplier's UN38.3 report received
