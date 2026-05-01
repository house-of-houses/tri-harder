# Tri Harder

A progressive, lab-based journey from basic electronics to building
a complete Eurorack VCO (Voltage Controlled Oscillator). Personal
learning project by a software engineer picking up analog circuit
design. Shared in case others find it useful.

## What You'll Learn

Starting from Ohm's law and ending with a tuned V/Oct VCO, these
labs build intuition one circuit block at a time: voltage dividers,
RC timing, op-amp fundamentals, integrators, comparators with
hysteresis, and finally how they combine into an oscillator with
exponential pitch control and waveshaping.

## Prerequisites

### Equipment

- Bench power supply (dual rail, at least +/-12 V)
- Oscilloscope (2-channel minimum)
- Multimeter
- Breadboard + jumper wires

### Software

- [Falstad Circuit Simulator](https://www.falstad.com/circuit/)
- [LTspice](https://www.analog.com/en/resources/design-tools-and-calculators/ltspice-simulator.html)

## Lab Progression

| # | Lab | Key Concept | Sim |
|---|-----|-------------|-----|
| 01 | Voltage Divider | Ohm's law, node voltages | Falstad |
| 02 | RC Circuits | Charge/discharge, time constants | Falstad |
| 03 | Op-Amp Basics | Inverting/non-inv, virtual ground | Falstad |
| 04 | Integrator | Constant current to linear ramp | Falstad |
| 05 | Schmitt Trigger | Positive feedback, hysteresis | Falstad |
| 06 | Relaxation Osc | Integrator + Schmitt = oscillation | Falstad |
| 07 | Triangle-Core VCO | Bidirectional ramp, tri+sq out | F+LT |
| 08 | Expo Converter | BJT Vbe-Ic, matched pairs, tempco | LTspice |
| 09 | Waveshaping | Tri-to-sine, PWM | F+LT |
| 10 | Full Discrete VCO | Combine 07-09, V/Oct tuning | LTspice |
| 11 | CEM3340 VCO | Chip internals, outputs, sync | LTspice |

## Phases

**Phase 1 -- Discrete Triangle-Core VCO (Labs 01-10)**
Build a working VCO from first principles using discrete op-amps
and transistors. Each lab introduces one building block; by lab 10
they all come together into a tuned, playable oscillator.

**Phase 2 -- CEM3340 / AS3340 VCO (Lab 11)**
Rebuild the VCO around the classic CEM3340 (or AS3340 clone) chip.
Understand what the IC does internally and how to design the
surrounding support circuitry.

## Repo Structure

```text
tri-harder/
  labs/
    01-voltage-divider/
      breadboard/        # photos, notes
      falstad/           # sim files
    ...
    07-triangle-core-vco/
      breadboard/
      falstad/
      ltspice/           # .asc schematics
    ...
  bom/                   # bill of materials
  docs/                  # planning docs
  spice-models/          # third-party SPICE models
```

## Resources

- [Moritz Klein -- VCO series (YouTube)](https://www.youtube.com/playlist?list=PLHeL0JWdJLvT1PAqW4TtvxtRoXyk741WM)
- [Aaron Lanterman -- Lantertronics ECE lectures](https://lanterman.ece.gatech.edu/)
- [Rene Schmitz -- Expo converter tutorial](https://www.schmitzbits.de/expo_tutorial/)
- [Electric Druid -- CEM3340 article](https://electricdruid.net/cem3340-vco-voltage-controlled-oscillator-designs/)
- [Erica Synths -- mki x es.EDU VCO manual](https://web.archive.org/web/20241014024000/https://www.ericasynths.lv/media/VCO_MANUAL_v2.pdf)
- [North Coast Synthesis -- VCO design notes](https://northcoastsynthesis.com/news/)
- [Kassutronics -- VCO build](https://kassu2000.blogspot.com/2018/06/vco.html)

A broader curated reading and viewing list, ordered by phase to match the
labs, lives in [docs/resources.md](docs/resources.md).
