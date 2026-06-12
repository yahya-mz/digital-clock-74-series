# Advanced Digital Clock — Pure 74-Series Logic (Proteus)

A fully functional **24-hour digital clock** designed and simulated entirely with **discrete 74-series logic ICs** in **Proteus 8** — **no microcontroller, no firmware, no programmable memory**. Every behavior (timekeeping, mode control, calendar, stopwatch) is built from gates, flip-flops, counters, decoders, and multiplexers.

Final project for the **Logic Circuits** course, Department of Computer Engineering, **Ferdowsi University of Mashhad** (Instructor: Dr. Ershadi-Nasab).

![Proteus](https://img.shields.io/badge/Proteus-8.x-002b5c)
![Logic](https://img.shields.io/badge/Design-74xx%20Discrete%20Logic-1a7f37)
![No MCU](https://img.shields.io/badge/Microcontroller-None-cf222e)

---

## Overview

The system displays **HH:MM:SS** on six common-anode seven-segment displays and is driven by a small **finite-state machine (FSM)** that switches between display and setting modes using only three pushbuttons. On top of the core clock, it adds a self-aware **calendar** (with automatic 30/31-day and leap-year handling) and a **stopwatch with lap**, plus an ambient-light **auto-dimming** feature.

The whole design is organized into reusable **sub-circuits** so the top sheet stays readable, and all interconnections use **named net labels** instead of long wires.

---

## Features

### Core (required)
- **24-hour time** — `HH:MM:SS` on six 7-segment displays.
- **Smart FSM interface** — three buttons (`Mode`, `Up/Set`, `Reset`) cycle through *Display -> Set Hour -> Set Minute* modes.
- **Blinking set mode** — the field being edited blinks while `Up` increments it.
- **Short vs. long press** — holding `Reset` for ~2 s triggers a full reset, distinct from a short press.

### Optional modules (implemented)
- **Complex Calendar** — day/month tracking with automatic detection of 30-day months (4, 6, 9, 11) vs. 31-day months.
- **Stopwatch & Lap** — lap capture that **freezes** the display while counting continues in the background, then returns to live time.

### Bonus
- **Leap-year logic** — Esfand (12th month) automatically switches between 29 and 30 days.
- **Auto-Dimming** — seven-segment brightness adapts to ambient light using an **LDR** and a **PWM**-based dimming technique.

> Alarm/Snooze (the third optional module) was intentionally not implemented, since only two of the three optional modules were required.

---

## Design Constraints

This project deliberately works within tight rules to keep the focus on pure logic design:

- No microcontroller (Arduino, 8051, AVR ...) and no ROM/EPROM for the core logic.
- Only basic gates, flip-flops, counters, decoders, multiplexers, and 74-series ICs.
- Counter/FSM logic built **by hand** (no built-in Proteus counter modules).
- All connections done with **labels**, and the design is split into **sub-circuits**.

---

## Architecture

The design is split into ~12 functional modules (across ~22 sub-circuit instances):

| # | Module | Role |
|---|--------|------|
| 1 | Clock generator / divider | Produces the 1 Hz tick and a slower blink clock (`CLK`, `CLK_SLOW`) |
| 2 | Seconds counter | 00-59 BCD counter -> tens/units digits |
| 3 | Minutes counter | 00-59, advanced by the seconds carry |
| 4 | Hours counter | 00-23 with 24-hour rollover via a comparator |
| 5 | FSM / mode controller | Switches Display / Set-Hour / Set-Minute / Calendar / Stopwatch |
| 6 | Button conditioning | Debounce + edge + long-press detection (Schmitt triggers) |
| 7 | Blink / set logic | Blinks the field under edit, gates the `Up` pulses |
| 8 | Display mux + decoder | Selects time / calendar / stopwatch and drives the 7-segments |
| 9 | Calendar | Day/month counters with 30/31-day detection |
| 10 | Leap-year unit | Esfand 29/30-day selection |
| 11 | Stopwatch / Lap | 0.1 s timing with freeze-on-lap |
| 12 | Auto-dimming | LDR + PWM brightness control |

---

## ICs Used

| IC | Function in this design |
|----|--------------------------|
| 74xx160 / 74xx161 | Decade / binary counters (time, calendar, stopwatch) |
| 74HC14 | Schmitt-trigger inverter — clock oscillator & button debounce |
| 74LS47 | BCD -> 7-segment decoder/driver |
| 74HC74 / 74LS74 | D flip-flops — FSM state & control |
| 74LS112 | JK flip-flop |
| 74HC85 | 4-bit magnitude comparator — rollover / value matching |
| 74HC153 / 74HC157 | Multiplexers — display source selection |
| 74HC125 | Tri-state buffers — bus sharing between modes |
| 74HC173 / 74HC374 | D registers / latches |
| 74HC08 / 74HC04 / 7410 | AND / NOT / NAND glue logic |
| 74HC238 / 74LS138 / 74HC147 | Decoders / priority encoder |
| 7SEG-COM-ANODE x6 | Time / value display |
| LDR | Ambient light sensing for auto-dimming |

---

## Signal Naming Convention

To keep the schematic readable, nets follow a consistent scheme:

- `AN_<FIELD>_D` / `AN_<FIELD>_Y` -> the **tens** (D) and **units** (Y) digit of a field (e.g. `AN_HOUR_D`, `AN_SEC_Y`).
- `CLK_<FIELD>_IN` -> the clock feeding each counter (`CLK_HOUR_IN`, `CLK_MONTH_IN`).
- `MODE_*` -> FSM / mode lines (`MODE_RAW`, `MODE_DB`, `MODE_DISP`, `MODE_DISP_CAL`).
- `RESET_RAW` / `RESET_LONG`, `UP_PULSE` -> conditioned button signals.
- `LAP1` / `LAP_EN`, `LEAP_YEAR`, `M_4 / M_6 / M_9 / M_11` -> stopwatch & calendar control lines.

---

## Getting Started

1. Install **Proteus 8.0** or newer.
2. Open `final_project.pdsprj`.
3. Press **Play** to start the simulation.
4. Use the on-sheet buttons:

| Button | Short press | Long press (~2 s) |
|--------|-------------|-------------------|
| **Mode** | Cycle to the next mode | — |
| **Up / Set** | Increment the selected (blinking) field | — |
| **Reset** | — | Reset the clock |

---

## Repository Structure

```
.
├── final_project.pdsprj     # Proteus project (open this)
├── docs/
│   └── schematic.png        # top-level schematic screenshot
└── README.md
```

> Tip: drop a screenshot of the top sheet into `docs/schematic.png` so it shows up here:
>
> `![Schematic](docs/schematic.png)`

---

## Deliverables / Report

The accompanying report covers:
- Truth tables for the combinational blocks.
- The **state diagram** of the button/mode FSM.
- The full list of ICs used.

---

## Author

**Yahya Mohammadzadeh** — Computer Engineering, Ferdowsi University of Mashhad
[github.com/yahya-mz](https://github.com/yahya-mz)
