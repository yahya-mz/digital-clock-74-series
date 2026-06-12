# Advanced Digital Clock — Pure 74-Series Logic (Proteus)

A fully functional **24-hour digital clock** designed and simulated entirely with **discrete 74-series logic ICs** in **Proteus 8** — no microcontroller, no firmware, no programmable memory. Every behavior is built from gates, flip-flops, counters, decoders, and multiplexers.

Final project for the **Logic Circuits** course, Department of Computer Engineering, **Ferdowsi University of Mashhad**.

![Proteus](https://img.shields.io/badge/Proteus-8.x-002b5c)
![Logic](https://img.shields.io/badge/Design-74xx%20Discrete%20Logic-1a7f37)
![No MCU](https://img.shields.io/badge/Microcontroller-None-cf222e)

![Schematic](https://github.com/yahya-mz/digital-clock-74-series/blob/main/docs/schematic.png)

---

## Features

### Core
- **24-hour time** — `HH:MM:SS` on six 7-segment displays.
- **Smart FSM** — three buttons (`Mode`, `Up/Set`, `Reset`) cycle through *Display → Set Hour → Set Minute*.
- **Blinking set mode** — the field being edited blinks while `Up` increments it.
- **Short vs. long press** — holding `Reset` for ~2 s triggers a full reset.

### Optional (implemented)
- **Complex Calendar** — day/month tracking with automatic 30/31-day detection.
- **Stopwatch & Lap** — freezes the display on lap while counting continues in the background.

### Bonus
- **Leap-year logic** — Esfand automatically switches between 29 and 30 days.
- **Auto-Dimming** — brightness adapts to ambient light via **LDR** + **PWM**.

---

## Design Constraints

- No microcontroller (Arduino, 8051, AVR …) and no ROM/EPROM.
- Only gates, flip-flops, counters, decoders, multiplexers, and 74-series ICs.
- FSM logic built **by hand** — no built-in Proteus counter modules.
- All connections via **net labels**; logic split into **sub-circuits**.

---

## Architecture

~12 functional modules across ~22 sub-circuit instances:

| # | Module | Role |
|---|--------|------|
| 1 | Clock generator | 1 Hz tick + slow blink clock |
| 2 | Seconds counter | 00–59 BCD |
| 3 | Minutes counter | 00–59, clocked by seconds carry |
| 4 | Hours counter | 00–23 with 24-hour rollover |
| 5 | FSM / mode controller | Display / Set-Hour / Set-Minute / Calendar / Stopwatch |
| 6 | Button conditioning | Debounce + long-press (Schmitt triggers) |
| 7 | Blink / set logic | Blinks the field under edit |
| 8 | Display mux + decoder | Drives six 7-segments |
| 9 | Calendar | Day/month with 30/31-day detection |
| 10 | Leap-year unit | Esfand 29/30-day logic |
| 11 | Stopwatch / Lap | 0.1 s resolution, freeze-on-lap |
| 12 | Auto-dimming | LDR + PWM brightness control |

---

## ICs Used

| IC | Role |
|----|------|
| 74xx160 / 74xx161 | Decade / binary counters |
| 74HC14 | Schmitt-trigger inverter — oscillator & debounce |
| 74LS47 | BCD → 7-segment decoder/driver |
| 74HC74 / 74LS74 | D flip-flops — FSM state |
| 74LS112 | JK flip-flop |
| 74HC85 | Magnitude comparator — rollover detection |
| 74HC153 / 74HC157 | Multiplexers — display source selection |
| 74HC125 | Tri-state buffers |
| 74HC173 | D registers / latches |
| 74HC08 / 74HC04 / 7410 | AND / NOT / NAND glue logic |
| 74HC238 / 74HC147 | Decoder / priority encoder |
| 7SEG-COM-ANODE ×6 | Display |
| LDR | Ambient light sensing |

---

## Getting Started

1. Open `final_project.pdsprj` in **Proteus 8.0+**.
2. Press **Play**.
3. Use the on-sheet buttons:

| Button | Short press | Long press (~2 s) |
|--------|-------------|-------------------|
| **Mode** | Next mode | — |
| **Up / Set** | Increment blinking field | — |
| **Reset** | — | Full reset |

---

## Author

**Yahya Mohammadzadeh** — Computer Engineering, Ferdowsi University of Mashhad
[github.com/yahya-mz](https://github.com/yahya-mz)
