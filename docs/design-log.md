# Design Log

A running record of design changes and the reasoning behind them. Newest entries at the top.
Each entry: **what** changed, **why**, and anything to **watch** later.

---

## 2026-10-27 — Repository organised

**What**
- Restructured the repo into `hardware/` (KiCad project + libraries), `firmware/` (placeholder), and `docs/`.
- Added project-specific library tables (`hardware/fp-lib-table`, `hardware/sym-lib-table`) using `${KIPRJMOD}` paths, so the project opens on any machine without editing global KiCad settings.
- Bundled the MX hot-swap socket 3D model (`hardware/lib/3d/MX-Hotswap-Socket.step`, MIT, from ai03's MX_V2) and pointed all switch footprints at it. Previously the path was `./MX-Hotswap-Socket.step`, which didn't resolve, so sockets were missing from the 3D view.
- Moved local-only files (MX_V2 library download, SnapMagic Pico footprint, old backups) into `_archive/`, which is git-ignored.

**Why**
- Keeps the hardware self-contained and portable, and makes room for firmware alongside it.

**Watch**
- If KiCad warns about duplicate library nicknames, remove the old `FootprintLibrary` / `KeyboardSymbolLibrary` entries from the *global* library tables (Preferences → Manage Libraries). The project tables now provide them.

---

## 2026-10-27 — Enclosure: custom 3D-printed case

**Decision**
- Keep the current board outline (344.6 × 135 mm, Pico on the right) and design a custom 3D-printed case around the existing 6 × M3 mounting holes.

**Why**
- Chance to learn 3D CAD and printing; avoids reworking the board to fit the standard 60% (GH60) outline.

**Watch**
- The case (~355 mm wide) is larger than most printer beds (~250 mm). Design it to print in two halves.
- A switch plate is still needed. Hot-swap sockets don't hold switches mechanically. Option: order an FR4 plate as a second "PCB".

---

## 2026-10-27 — GND fills removed

**What**
- Deleted the GND copper pours on F.Cu and B.Cu. The Pico's GND pins are now unconnected on this board.

**Why**
- Matrix current flows Pico pin → row → switch → diode → column → Pico pin. Nothing returns through GND, and scanning runs at ~kHz, so the pours had no electrical function. Removing them keeps the board readable while routing.

**Watch**
- The Pico gets ground from USB. If anything is ever added that needs GND (LEDs, an I²C display, etc.), route GND or re-add a pour.

---

## 2026-10-27 — Layout rework: switches flipped, routing cleared, diodes relocated

**What**
1. Flipped all 61 MX hot-swap switch footprints (left/right flip, same as KiCad's **F**). Sockets are now on B.Cu and the switch pin holes match the standard MX pattern.
2. Deleted all routing: 1,044 track segments and 108 vias.
3. Moved every diode to sit under its own switch: B.Cu, centred 5 mm below the stem hole, horizontal, cathode (pad 1) toward +x.

**Why**
1. *Critical bug.* The MX_V2 library changed in 2024 so hot-swap footprints ship with the socket on top, and its README says to flip them. Unflipped, the sockets would be soldered on the top side with mirrored pin holes, and switches could not be installed.
2. Flipping moves the socket pads onto B.Cu, so the old bottom-layer traces ran straight through them (44 shorts). A full reroute was needed anyway for item 3.
3. *Layout fundamental: keep local nets local.* All 61 diodes had been in one strip along the bottom edge, giving each switch its own 50–90 mm trace (~9.7 m of copper in total). With each diode under its switch, only the shared rows (5) and columns (14) travel any distance. Each key becomes an identical unit cell, which makes routing, review and debugging much simpler.

**Watch**
- Next: route rows horizontally on one layer and columns vertically on the other.
- One Row 1 trace previously passed within 0.25 mm of an SW60 stabilizer hole. Keep clear when rerouting.
- Run DRC to zero errors and check the 3D viewer (Alt+3) before ordering.

---

## 2026-10-27 — Design review (v1)

Full review: [`design-review-2026-10-27.md`](design-review-2026-10-27.md).

**Key findings**
- Matrix correct: 5 × 14, 61 keys, no duplicate positions, one diode per key, consistent direction (row → column).
- All nets routed with no shorts or clearance violations at 0.2 mm (custom script, not KiCad DRC).
- Critical: switch footprints not flipped (see above).
- Diodes far from switches (see above).
- Board is larger than the key area (Pico on the side plus wide margins).

---

## 2026-02 — Initial design

- 61-key ANSI 60% layout, 5 × 14 matrix.
- MX hot-swap footprints (ai03 MX_V2), 1N4148-style SOD-123 diodes, Raspberry Pi Pico mounted on the PCB.
- Pin map: rows GP1–GP5; columns GP11–GP22, GP26, GP27. Free: GP0, GP6–GP10, GP28.
- Firmware scan direction is set by the diodes: drive one column LOW, read rows with internal pull-ups.
