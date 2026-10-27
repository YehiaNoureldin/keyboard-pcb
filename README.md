# keyboard-pcb

A from-scratch 61-key mechanical keyboard: custom PCB designed in KiCad, a Raspberry Pi Pico as the controller, and firmware written by hand in C.

**Goals:** learn PCB layout fundamentals, then firmware fundamentals (GPIO, matrix scanning, debouncing, USB HID, layers).

## Repository structure
```
keyboard-pcb/
├── hardware/      KiCad 9 project: schematic, PCB, libraries, 3D models
├── firmware/      C firmware (Pico SDK), in progress
└── docs/          Design log and design reviews
```

## Hardware at a glance
- ANSI 60% layout, 61 keys, 5 × 14 matrix with one diode per key
- MX hot-swap sockets, Raspberry Pi Pico mounted on the PCB
- 2-layer board, 344.6 × 135 mm, 6 × M3 mounting holes for a 3D-printed case

Details: [`hardware/README.md`](hardware/README.md)

## Status
- [x] Schematic
- [x] Placement: switches flipped (sockets on bottom), diodes under each switch
- [ ] Routing (rows and columns)
- [ ] DRC clean, order PCBs
- [ ] 3D-printed case and switch plate
- [ ] Firmware ([roadmap](firmware/README.md))

## Documentation
- [Design log](docs/design-log.md): every design change and the reasoning behind it
- [Design review, 2026-10-27](docs/design-review-2026-10-27.md)

## Credits
Switch footprints and the socket 3D model are from [ai03's MX_V2 library](https://github.com/ai03-2725/MX_V2) (MIT License).
