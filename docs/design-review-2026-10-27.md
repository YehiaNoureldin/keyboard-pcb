# Keyboard PCB — Design Review v1 (2026-10-27)

Reviewed: `Keyboard.kicad_sch` / `Keyboard.kicad_pcb` (KiCad 9), footprint library, MX_V2 library.
Change history and decisions: [`design-log.md`](design-log.md).

## Status: not ready to order (rework in progress)

### 1. Critical: switch footprints not flipped. Fixed 2026-10-27
- MX_V2 README (2024-08-15 change): hot-swap footprints ship with the socket side on top; every switch footprint must be flipped so the socket is on the bottom.
- Was: all 61 switches on F.Cu, socket pads on top, switch pin holes mirrored.
- Fixed: all 61 flipped (left/right, same as KiCad's F). Sockets on B.Cu, standard MX pin pattern.
- Habit: check the 3D viewer (Alt+3) before every order.

### 2. Layout fundamental: diodes far from switches. Placed 2026-10-27
- Was: all 61 diodes in one strip along the bottom edge. Switch-to-diode distance: median 53 mm, max 92 mm. ~9.7 m of total copper.
- Done: all tracks and vias deleted. Each diode now on B.Cu, centred 5 mm below its switch's stem hole, horizontal, cathode (pad 1) toward +x.
- Next: route rows horizontally on one layer and columns vertically on the other.

### 3. Board size / Pico placement. Decided 2026-10-27
- Board 344.6 × 135 mm; key area 285.75 × 95.25 mm (standard ANSI 60%, 61 keys).
- Decision: keep this outline and design a custom 3D-printed case around the 6 × M3 holes.

### 4. GND fills. Removed 2026-10-27
- GND pours on both layers had no electrical function for the matrix and were removed.

### 5. Minor
- Watch the clearance to the SW60 stabilizer hole when rerouting Row 1.

## What's good
- Matrix: 5 rows × 14 columns, 61 keys, no duplicate positions, one diode per key, diode direction consistent.
- Schematic has no-connect flags on unused pins (25) and global labels for rows/columns.
- Proven footprint library, hot-swap sockets, free pins kept for debugging.

## Rework plan
1. ~~Flip all switch footprints (socket to bottom).~~
2. ~~Delete all tracks.~~
3. ~~Place each diode next to its socket, bottom side, same orientation.~~
4. ~~Remove GND fills.~~
5. ~~Decide the Pico location and the board outline.~~ Custom case.
6. Route rows horizontally on one layer and columns vertically on the other.
7. Run DRC to 0 errors, check the 3D view, then order.
