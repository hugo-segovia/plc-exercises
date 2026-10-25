# PLC Exercises – CODESYS (IEC 61131-3)

Hi! I'm **Hugo Segovia**, a student of the Higher Vocational Degree in Electrical Systems and Automation (CFGS) in Valencia, Spain.
This repository collects the PLC programming exercises I solve on my own, alongside my studies, to build real automation skills.

**Goal:** start a Bachelor's degree in Electrical Engineering – Automation & Industrial IoT at TH Mannheim (Germany) in 2028.

## Tools
- **CODESYS V3.5** (CODESYS Control Win V3 x64, simulation mode)
- Languages: **Ladder Diagram (LD)**, **SFC**, **Structured Text (ST)**
- Design methods: **GRAFCET** (IEC 60848)
- Wiring diagrams: CADe_SIMU
- Also used: OpenPLC, Siemens LOGO! Soft Comfort, Factory I/O

## How each exercise is documented
Every folder contains:
1. **README** – the task description (real-world scenario), I/O table and how the program works
2. **Wiring diagram** (PDF) – power and control circuits
3. **GRAFCET** (from exercise 4 onwards)
4. **Program** – CODESYS project file and PLCopen XML export
5. **Test table** – the situations I tested in simulation, including fault cases
6. **Screenshot** of the program running online

Safety principle applied in all exercises: stop buttons, emergency stops and overload relays are wired **normally closed (NC)** (fail-safe).

## Exercises

| # | Exercise | Concepts | Status |
|---|---|---|---|
| 01 | [Start/stop of an extractor fan](01-start-stop/) | Self-holding, overload relay, pilot lights | ✅ Basic · ⏳ Extension |
| 02 | Reversing a sliding door motor | Interlocking (software + hardware), limit switches | ⏳ |
| 03 | Star-delta start of a compressor | Timers (TON), sequence, interlocking | ⏳ |
| 04 | Pedestrian traffic light | GRAFCET, sequences, blinking | ⏳ |
| 05 | Part counter on a packaging line | Counters (CTU), edge detection | ⏳ |
| 06 | Conveyor with ejector cylinder | GRAFCET → LD (set/reset) and SFC, emergency stop, fault timeouts | ⏳ |
| 07 | Sump with two alternating pumps | Hysteresis, pump alternation, alarms | ⏳ |
| 08 | Exercise 6 in Structured Text | ST, CASE state machine | ⏳ |

## Contact
- LinkedIn: https://www.linkedin.com/in/hugo-segovia-nevado/

