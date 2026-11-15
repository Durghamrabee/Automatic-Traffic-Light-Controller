# Automatic Traffic Light Controller

A two-direction traffic light controller written in Ladder Diagram for the Schneider Electric Zelio Logic SR3B262BD. The program cycles automatically through green, yellow, and all-red phases for directions A and B.

**Author** Durgham Rabee ECE

**Version** 1.0

## Project files

- [Zelio Soft 2 project](Automatic_Traffic_Light_Controller.zm2) — editable ladder program.
- [Program documentation](Automatic%20Traffic%20Light%20Controller.pdf) — ladder diagram, I/O assignments, timer settings, and LCD text block.

## Operating sequence

Turning on `I1` enables the system. A one-scan start pulse initiates the A green stage. The stage relays and timers advance the sequence, and the second all-red stage leads back to A green.

| Stage relay | Direction A | Direction B | Timer | Duration |
| --- | --- | --- | --- | --- |
| `M2` | Green | Red | `T1` | 10 s |
| `M3` | Yellow | Red | `T2` | 3 s |
| `M4` | Red | Red | `T3` | 2 s |
| `M5` | Red | Green | `T4` | 10 s |
| `M6` | Red | Yellow | `T5` | 3 s |
| `M7` | Red | Red | `T6` | 2 s |

The timer settings total 30 seconds per cycle. Each direction change includes a 2-second interval with both signals red.

## Input and outputs

| Address | Function |
| --- | --- |
| `I1` | System enable |
| `Q1` | Direction A green |
| `Q2` | Direction A yellow |
| `Q3` | Direction A red |
| `Q4` | Direction B green |
| `Q5` | Direction B yellow |
| `Q6` | Direction B red |

## Simulate in Zelio Soft 2

1. Download the `.zm2` project and open it in Zelio Soft 2.
2. Switch to **Simulation** mode and start the simulation.
3. Turn on `I1` and watch `Q1`–`Q6` through a full cycle. Check that both red outputs are active during each all-red phase and that the sequence returns to A green.
4. Toggle `I1` off and back on to observe how the program responds to a stop and restart.
5. With `I1` on, open the simulated LCD to inspect the author text configured in block `X1`.
