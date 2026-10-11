# Decisions

One entry per decision, newest at the bottom. Format: date, decision, why, where it lives.
Reversing a decision is a new entry that points to the old one.

---

### 2026-10-02 · First test phone: iPhone 18 Pro on iOS 27
Front dial sets real aperture (f/1.48 to f/4, main camera only). Other iPhones are supported.
Where: `SPEC.md` Target.

### 2026-10-02 · Fixed-aperture cameras: no remapping of the front dial to ISO
The guide maps the front dial to ISO on fixed-aperture phones. Instead, ISO has its own hold
button on every model, and Aperture priority is dropped from the mode cycle when
`minLensAperture == maxLensAperture` (this includes the 18 Pro's ultra wide and telephoto).
Why: one consistent control layout on every phone and lens.
Where: `SPEC.md` Target and Dial behavior by mode.

### 2026-10-02 · ISO and WB hold buttons added (D9, D10)
Holding ISO or WB repurposes the rear dial. The firmware sends raw press/release; the app
resolves the combinations, so changing a button's job never needs a firmware update.
Consequence: all of D0-D10 are used; any further button needs an I²C port expander.
Where: `SPEC.md` Controls and Pins. Not yet in firmware or `PROTOCOL.md` 0.1.0 (see PLAN 3.6).

### 2026-10-02 · Mode set: M / A / S / Auto, plus exposure compensation
A and Auto put EV comp on the rear dial; S puts it on the front dial. Front dial press = Auto
with EV comp reset to 0. Rear dial press = cycle lenses.
Where: `SPEC.md`.

### 2026-10-02 · Provisional feel defaults (revisit in Phase 5)
Nikon direction (rear clockwise = faster shutter), one detent = one third stop, no
acceleration, lists stop at the ends. Where: `SPEC.md` Feel defaults.

### 2026-10-02 · Phone mount: spring clamp on a printed 1/4-20 stud
Works with any case; cage and MagSafe go to V2. Where: `SPEC.md` Form factor, `V2.md`.

### 2026-10-02 · V2 cuts
Manual focus, Auto ISO in M, user ISO in A/S, fine WB, extra buttons, acceleration, wrap-around,
left-handed grip, remapping. Where: `V2.md`.

### 2026-10-05 · Toolchain: Arduino IDE + Seeed nRF52 Boards (non-mbed) + Bounce2
The guide's recommended core; Bluefruit examples apply. Never the mbed-enabled core.
Where: fw `CLAUDE.md`, fw README A1-A2.

### 2026-10-05 · Connection interval 15-30 ms, not the guide's 7.5-15 ms
iOS rejects intervals below 15 ms for a non-HID accessory; Apple requires max ≥ min + 15 ms.
Where: `PROTOCOL.md` Connection parameters, `dial_config.h`.

### 2026-10-05 · Protocol 0.1.0 details
128-bit UUIDs `188D000x-8E0D-4A9D-9B04-830DCBEB93B8`; 4-byte events; dial deltas coalesced
over 15 ms; mode press 400-800 ms sends nothing; no pairing/bonding in 0.1.0; Feedback
command `0x01` LED override. Dials don't wake from sleep, only buttons do.
Where: `PROTOCOL.md`.

### 2026-10-05 · Tag fw-0.1.0 only after the Phase 3 hardware checklist passes
Where: fw `CLAUDE.md`, fw README.

### 2026-10-05 · Project tracking lives in `dial-app/docs/`
PLAN, STATUS, DECISIONS and LOG sit next to `SPEC.md` and `PROTOCOL.md` in `dial-app`, so a
Claude Chat project connected to that one GitHub repo sees spec, protocol and status together.
Both repos' `CLAUDE.md` point here; `/status`, `/checkpoint` and `/gaps` exist in both repos.
