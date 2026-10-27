# Firmware

Keyboard firmware written from scratch in C with the [Raspberry Pi Pico SDK](https://github.com/raspberrypi/pico-sdk).
The goal is to learn firmware fundamentals, so everything except the USB stack (TinyUSB, bundled with the SDK) is written by hand.

## Roadmap
- [ ] **Toolchain**: Pico SDK + CMake build, blink the onboard LED, flash over USB (BOOTSEL)
- [ ] **Debug output**: `printf` over UART1 (GP8 TX / GP9 RX)
- [ ] **GPIO input**: read a single key
- [ ] **Matrix scan**: drive one column LOW at a time, read rows with internal pull-ups, settle delay before reading
- [ ] **Debounce**: reject switch chatter
- [ ] **USB HID**: report key presses to the host with TinyUSB
- [ ] **Keymap & layers**: Fn layer, remapping
- [ ] **SWD debugging**: second Pico as a debug probe

## Hardware interface
| Signal | Pico pins | Direction |
| --- | --- | --- |
| Columns 1–12 | GP11–GP22 | Output (driven LOW one at a time) |
| Columns 13–14 | GP26, GP27 | Output |
| Rows 1–5 | GP1–GP5 | Input, internal pull-up (LOW = key pressed) |
| UART1 debug | GP8 (TX), GP9 (RX) | |

The diodes point from rows to columns, so the scan must drive **columns** and read **rows**.

See [`../hardware/`](../hardware/) for the schematic and [`../docs/design-log.md`](../docs/design-log.md) for design decisions.
