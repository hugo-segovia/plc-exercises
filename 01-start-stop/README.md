# 01 – Start/stop of an extractor fan

## Scenario
The fume extractor of a welding workshop is started and stopped from a push-button station next to the door.

## I/O table

| Symbol | Description | Type |
|---|---|---|
| S0 | Stop push button (red) | Input, NC |
| S1 | Start push button (green) | Input, NO |
| F1 | Overload relay auxiliary contact (95-96) | Input, NC |
| KM1 | Motor contactor | Output |
| H1 | Green pilot light "fan running" | Output |
| H2 | Red pilot light "overload tripped" | Output |

## How it works
- Pressing **S1** starts the motor; it keeps running after releasing the button (self-holding contact of KM1).
- Pressing **S0** stops the motor.
- If the overload relay **F1** trips, the motor stops and **H2** turns on. While F1 is tripped, S1 cannot restart the motor.
- **H1** is on whenever KM1 is energised.
- If S0 and S1 are pressed at the same time, **stop has priority**.

Logic (equivalent):
```
KM1 := (S1 OR KM1) AND S0 AND F1;
H1  := KM1;
H2  := NOT F1;
```

## Files
- `diagram.pdf` – power and control wiring
- `01-start-stop.project` – CODESYS project
- `01-start-stop.xml` – PLCopen XML export (readable)
- `screenshot.png` – program running in simulation

## Test table

| # | Action | Expected result | OK |
|---|---|---|---|
| 1 | Simulation start (S0 = TRUE, F1 = TRUE, S1 = FALSE) | KM1 off, H1 off, H2 off |  |
| 2 | S1 → TRUE, then FALSE | KM1 on and stays on, H1 on |  |
| 3 | S0 → FALSE | KM1 off, H1 off |  |
| 4 | Motor running, F1 → FALSE (trip) | KM1 off, H2 on |  |
| 5 | F1 still FALSE, S1 → TRUE | KM1 stays off |  |
| 6 | S0 = FALSE and S1 = TRUE at the same time | KM1 off (stop has priority) |  |

## Extension (to do)
- Emergency stop **S9** (NC): stops the motor in any situation; after releasing it, the motor must not restart until S1 is pressed again.
- **H2** blinks (1 s on / 1 s off) after an emergency stop and stays steady after an overload trip (oscillator with two TON timers).
