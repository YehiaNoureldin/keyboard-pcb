# Keyboard Personal Project

A from-scratch 61-key (ANSI 60%) mechanical keyboard: custom PCB designed in KiCad 9, a Raspberry Pi Pico as the controller, and firmware written by hand in C with the Pico SDK.

Goals: learn PCB layout fundamentals, then firmware fundamentals (GPIO, matrix scanning, debouncing, USB HID with TinyUSB, layers).

## Hardware
- 5 × 14 key matrix, one SOD-123 diode per key (current flows row → column)
- MX hot-swap sockets on the bottom side; diodes on the bottom, under each switch
- Raspberry Pi Pico mounted directly on the PCB
- 6 × M3 mounting holes for a 3D-printed case

## Pin map
| Signal | Pico pins |
| --- | --- |
| Rows 1–5 | GP1–GP5 |
| Columns 1–12 | GP11–GP22 |
| Columns 13–14 | GP26, GP27 |
| Free | GP0, GP6–GP10, GP28 (UART1 on GP8/GP9 for debug output) |

Scanning: drive one column LOW, read rows with internal pull-ups.

## Repository layout
- `Keyboard/` — KiCad project (schematic, PCB)
- `FootprintLibrary.pretty/` — footprints used by the board
- `KeyboardSymbolLibrary.kicad_sym` — custom Pico symbol

## Status
- [x] Schematic
- [x] Switch footprints flipped (sockets on bottom), diodes placed under switches
- [ ] Route rows and columns
- [ ] DRC clean, order PCBs
- [ ] 3D-printed case and switch plate
- [ ] Firmware

## Credits
Switch footprints are from [ai03's MX_V2 library](https://github.com/ai03-2725/MX_V2) (MIT License).
