# Automatic Traffic Light Controller

A two-direction traffic light controller built in **Zelio Soft 2** using Ladder Diagram (LD). It alternates right of way between directions A and B, with an all-red clearance interval before each changeover.

| Project information | Details |
| --- | --- |
| Author | Durgham Rabee ECE |
| Version | 1.1 |
| Controller | Schneider Electric Zelio Logic SR3B262BD |
| Program cycle time | 20 ms (10 × 2 ms) |
| Ladder rows | 42 / 240 |
| Hardware input filter | Slow (3 ms) |
| Watchdog action | Inactive |
| Zx keys | Inactive |

## Project files

- [Zelio Soft 2 project](project/Automatic_Traffic_Light_Controller_V1.1.zm2) — editable Ladder Diagram program.
- [Full program report](docs/Automatic%20Traffic%20Light%20Controller%20V1.1.pdf) — module settings, ladder rungs, I/O assignments, timers, and LCD configuration.
- **[Ladder diagram, page 2](docs/ladder-v1.1-part-1.png)** · **[Ladder diagram, page 3](docs/ladder-v1.1-part-2.png)** · **[Ladder diagram, page 4](docs/ladder-v1.1-part-3.png)**

### Ladder logic preview

![Zelio ladder diagram, rungs 001–020](docs/ladder-v1.1-part-1.png)

![Zelio ladder diagram, rungs 021–040](docs/ladder-v1.1-part-2.png)

![Zelio ladder diagram, rungs 041–042](docs/ladder-v1.1-part-3.png)

## How the controller works

The `I1` enable input drives the run relay `M1`. On enable, `MB` holds the previous run state so `MC` can generate a one-scan start pulse. This pulse starts the A green stage. Stage relays `M2`–`M7` drive the lamps and their associated timers. When the second all-red timer finishes, the program returns to A green automatically.

| Stage | Direction A | Direction B | Relay | Timer | Preset |
| --- | --- | --- | --- | --- | --- |
| 1 | Green | Red | `M2` | `T1` | 10 s |
| 2 | Yellow | Red | `M3` | `T2` | 3 s |
| 3 | Red | Red | `M4` | `T3` | 2 s |
| 4 | Red | Green | `M5` | `T4` | 10 s |
| 5 | Red | Yellow | `M6` | `T5` | 3 s |
| 6 | Red | Red | `M7` | `T6` | 2 s |

The six **Function A** timers have a combined preset of **30 seconds** per cycle. Both red outputs are on during each **2-second** clearance interval.

## V1.1 — Master enable and reset

- `M1` enables all six stage SET commands, timer control coils and physical outputs.
- Normally closed `m1` contacts reset every stage relay `M2`–`M7` when `I1` is switched off.
- Function A timers reset when their control signals are removed.
- Restart begins at A green via the start pulse `MC`, after the controller has registered the OFF state.
- The nominal traffic cycle remains 30 seconds. The ladder expands from 37 to 42 rows.

## I/O and LCD

| Address | Function |
| --- | --- |
| `I1` | System enable |
| `Q1` | Direction A green |
| `Q2` | Direction A yellow |
| `Q3` | Direction A red |
| `Q4` | Direction B green |
| `Q5` | Direction B yellow |
| `Q6` | Direction B red |

Rung `R042` enables text block `X1` through coil `TX1` while `M1` is active. The block is configured to display **WISAM ECE, MARYAM ECE and BALSAM ECE** on the Zelio LCD.

## Simulate in Zelio Soft 2

1. Open the `.zm2` file, switch to **Simulation** mode, and start the simulation.
2. Turn on `I1` and watch `Q1`–`Q6` through a full cycle. Check that `Q3` and `Q6` are on during each all-red interval and that the sequence repeats from A green.
3. Switch `I1` off during each of the six stages. Verify all outputs and stage relays turn off and timers reset. Switch it on again and verify A green starts a fresh 10-second interval. Test rapid OFF/ON transitions that the PLC can register.
4. With `I1` on, open the simulated LCD to inspect the text from `X1`.

## Verification and safety

The sequence and timer values above describe the **documented PLC program**. The V1.1 report records successful functional simulation tests. The ladder has been statically reviewed; independent hardware testing and road-traffic commissioning are not claimed.

**Educational demonstration only.** The logic is not a certified traffic-signal controller and does not implement the independent fail-safe monitoring, fault-response architecture, or regulatory requirements needed for public-road deployment. The inactive watchdog setting must not be interpreted as an acceptable safety configuration for real traffic control.

## Author and rights

**Durgham Rabee** — Electronics & Communications Engineering. © 2026. All rights reserved unless otherwise stated.

[Engineering portfolio](https://github.com/Durghamrabee)
