# dial-app

iPhone camera app (SwiftUI, iOS 27) driven by the Bluetooth LE dial controller whose firmware
is in `~/dial-firmware`. Not started yet; the Xcode project arrives in Phase 4.

## Source of truth
- `SPEC.md`: product spec. Source of truth; `~/dial-firmware/SPEC.md` is a copy.
- `PROTOCOL.md`: BLE contract. Must stay byte-identical with `~/dial-firmware/PROTOCOL.md`.
  Any protocol change updates both copies, the firmware (`dial_config.h`, `protocol.h`) and the
  Swift constants.
- `V2.md`: everything cut from the MVP. Don't build from it before Phase 5 is done.

## Project tracking
The plan and status for the whole project (both repos) live in `docs/`:
`PLAN.md` (checklist by phase), `STATUS.md` (snapshot), `DECISIONS.md`, `LOG.md`.
- At the start of each session, read `docs/STATUS.md` (start with "Where we left off") and the
  current phase in `docs/PLAN.md`.
- After completing a step, tick it in `PLAN.md`, update `STATUS.md` (always including
  "Where we left off": doing, last done, exact next step and the file that describes it), and
  append a dated entry to `LOG.md`. Record any design choice in `DECISIONS.md`.
- Never tick a box unless the evidence exists (a file, a commit, a measured number). Steps that
  need hardware or the phone stay unticked until Diego reports the result.
- Commands: `/resume` (where we left off + the next step), `/status`, `/checkpoint`, `/gaps`.

## Working with Diego
- Diego is new to electronics. Hardware instructions go one step at a time: exact holes and pins,
  what he should see, what to report back. Define any term the first time it appears.
- He shares iPhone photos as `.HEIC` in `~/Downloads`; convert with `sips -s format jpeg` to view.
  Use his photos in explanations (annotate them when it helps).

## App rules (from the guide, for Phase 4)
- Minimum deployment iOS 27, Swift 6 strict concurrency, start from Apple's AVCam sample.
- Exposure changes run on the session queue inside `lockForConfiguration`, validated with
  `supportsExposureModeCustom`; `photoQualityPrioritization = .speed`.
- Pairing via AccessorySetupKit; `NSAccessorySetupBluetoothServices` uses the service UUID in
  upper case. No `external-accessory` background mode, and no background mode at all.
- Verify every iOS 27 API name in Xcode's documentation window; the aperture APIs are new.
