# 01 – Start/stop of an extractor fan

## Scenario
The fume extractor of a welding workshop is started and stopped from a push-button station next to the door. An emergency stop button can stop it in any situation.

## I/O table

| Symbol | Description | Type |
|---|---|---|
| S0 | Stop push button (red) | Input, NC |
| S1 | Start push button (green) | Input, NO |
| S9 | Emergency stop mushroom button (latching) | Input, NC |
| F1 | Overload relay auxiliary contact (95-96) | Input, NC |
| KM1 | Motor contactor | Output |
| H1 | Green pilot light "fan running" | Output |
| H2 | Red pilot light "overload tripped" | Output |
| H3 | Pilot light "emergency stop" (blinking) | Output |
| tT1, tT2 | TON timers (1 s each) for the blinking oscillator | Internal |
| KA1, KA2 | Internal bits of the oscillator | Internal |

## How it works
- Pressing **S1** starts the motor; it keeps running after releasing the button (self-holding contact of KM1).
- Pressing **S0** stops the motor.
- If the overload relay **F1** trips, the motor stops and **H2** turns on. While F1 is tripped, S1 cannot restart the motor.
- If the emergency stop **S9** is pressed, the motor stops immediately and **H3** blinks (1 s on / 1 s off). After releasing S9, the motor does **not** restart by itself: S1 must be pressed again.
- **H1** is on whenever KM1 is energised.
- Fail-safe wiring: S0, S9 and F1 are NC contacts, so they are TRUE in normal operation and the program uses normally open contacts for them.

Logic (equivalent in Structured Text):

    KM1 := (S1 OR KM1) AND S0 AND F1 AND S9;
    H1  := KM1;
    H2  := NOT F1;
    tT1(IN := NOT S9 AND NOT KA2, PT := T#1S);  KA1 := tT1.Q;
    tT2(IN := KA1, PT := T#1S);                 KA2 := tT2.Q;
    H3  := KA1 AND NOT KA2;

## Files
- `01-start-stop.project` – CODESYS project
- `screenshot.png` – program running in simulation

## Test table

| # | Action | Expected result | OK |
|---|---|---|---|
| 1 | Simulation start (S0 = TRUE, F1 = TRUE, S9 = TRUE, S1 = FALSE) | KM1 off, H1 off, H2 off, H3 off |  |
| 2 | S1 → TRUE, then FALSE | KM1 on and stays on, H1 on |  |
| 3 | S0 → FALSE | KM1 off, H1 off |  |
| 4 | Motor running, F1 → FALSE (trip) | KM1 off, H2 on |  |
| 5 | F1 still FALSE, S1 → TRUE | KM1 stays off |  |
| 6 | S0 = FALSE and S1 = TRUE at the same time | KM1 off (stop has priority) |  |
| 7 | Motor running, S9 → FALSE (emergency) | KM1 off, H3 blinks 1 s / 1 s |  |
| 8 | S9 → TRUE again (released) | KM1 stays off, H3 off |  |
| 9 | After step 8, S1 → TRUE | KM1 on |  |
| 10 | S9 = FALSE, S1 → TRUE | KM1 stays off |  |

## Versions
- v1 – basic start/stop with overload protection
- v2 – emergency stop S9 and blinking emergency light H3 (TON oscillator)
