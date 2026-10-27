# Hardware

KiCad 9 project for the keyboard PCB.

## Opening the project
Open `Keyboard.kicad_pro` in KiCad 9. Everything the project needs is in this folder:

| Path | Contents |
| --- | --- |
| `Keyboard.kicad_sch` | Schematic: 5 × 14 key matrix, diodes, Raspberry Pi Pico |
| `Keyboard.kicad_pcb` | PCB layout (2-layer) |
| `fp-lib-table`, `sym-lib-table` | Project library tables (paths relative to this folder) |
| `lib/FootprintLibrary.pretty/` | Footprints: MX hot-swap switches (1U–6.25U), Raspberry Pi Pico |
| `lib/KeyboardSymbolLibrary.kicad_sym` | Raspberry Pi Pico symbol |
| `lib/3d/` | 3D models used by the footprints |

## Board summary
- 61 keys, ANSI 60% layout, 5 × 14 matrix, one SOD-123 diode per key (current flows row → column)
- MX hot-swap sockets and diodes on the bottom side (B.Cu); switches and Pico on top
- Board outline 344.6 × 135 mm, 6 × M3 mounting holes
- 2 layers, 0.2 mm tracks, 0.6 / 0.3 mm vias

## Pin map
| Signal | Pico pins |
| --- | --- |
| Rows 1–5 | GP1–GP5 |
| Columns 1–12 | GP11–GP22 |
| Columns 13–14 | GP26, GP27 |
| Free | GP0, GP6–GP10, GP28 |

## Before ordering
1. Run the design rules check (Inspect → Design Rules Checker) with zero errors.
2. Check the 3D viewer (Alt+3): sockets and diodes on the bottom, switches and Pico on top.
3. Export Gerbers and the drill file into a `gerbers/` folder (git-ignored).

## Credits
Switch footprints and the hot-swap socket 3D model are from [ai03's MX_V2 library](https://github.com/ai03-2725/MX_V2) (MIT License).
