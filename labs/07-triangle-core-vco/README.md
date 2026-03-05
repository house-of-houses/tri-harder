# Lab 07 -- Triangle-Core VCO

## Goal

Build a proper triangle-core VCO with bidirectional current
steering, producing clean triangle and square wave outputs.
Transition from fixed-frequency oscillator to a
voltage-controllable design.

## Theory

Lab 06 drove the integrator from the Schmitt trigger's output
through a fixed resistor. This works but has limitations: the
resistor value sets a fixed frequency, and current sourcing is
asymmetric (Schmitt output swings between +/-Vsat but loading
varies).

### Current steering

A better approach uses **matched current sources** that
alternate direction. The Schmitt trigger controls which
current source is active:

1. When Schmitt is HIGH: current source pushes current into
   the integrator cap (ramps up).
2. When Schmitt is LOW: current sink pulls current from the
   integrator cap (ramps down).
3. Matched currents ensure symmetric triangle wave.

This can be implemented with:

- **Transistor pair** (2N3904/2N3906): PNP sources current,
  NPN sinks current, Schmitt output selects which is on.
- **OTA** (LM13700): transconductance amplifier provides
  voltage-controllable current directly.

### Voltage control

Changing the current magnitude changes the ramp rate and thus
the frequency, without affecting the triangle amplitude (set
by Schmitt thresholds). This separation of frequency and
amplitude control is fundamental to VCO design.

For V/Oct musical tracking, the current must double for each
volt increase. This exponential relationship is provided by
the expo converter (lab 08).

### Triangle vs sawtooth core

Triangle core: integrator ramps up and down symmetrically.
Triangle and square wave outputs are natural. Sine output
requires waveshaping (lab 09). No reset transient.

Sawtooth core: integrator ramps in one direction and resets
quickly. Sawtooth output is natural. Used in CEM3340 (lab 11).
Reset transient can cause timing jitter.

## Simulation

### Falstad (`falstad/triangle-core-vco.txt`)

Import via File > Import in the
[Falstad simulator](https://www.falstad.com/circuit/).

1. Two-op-amp core with transistor current steering.
2. Observe triangle wave at integrator output, square wave at
   Schmitt output.
3. Adjust the current source bias to change frequency.
4. Note how frequency changes without affecting triangle
   amplitude.

Also see the Falstad built-in triangle oscillator:
`e-triangle.html`.

### LTspice (`ltspice/triangle-core-vco.asc`)

Open in LTspice and run the simulation.

1. Uses `opamp2` (LTspice built-in) and 2N3904/2N3906.
2. Transient simulation: `.tran 0 50m 0 1u uic`.
3. Verify oscillation starts and stabilizes.
4. Probe the integrator output (triangle) and Schmitt output
   (square).
5. Measure frequency and amplitude.
6. Try changing the timing cap to shift frequency.

## Breadboard

### Components needed

- 1x TL072 dual op-amp
- 1x 2N3904 NPN transistor
- 1x 2N3906 PNP transistor
- 4x 10k ohm resistors
- 1x 100k ohm resistor
- 1x 10nF film capacitor (timing)
- 1x 10k trimpot (frequency adjust)
- 100nF bypass caps for power pins
- Bench power supply: +/-12V
- Oscilloscope

### Procedure

1. Wire the integrator (one TL072 half): 10nF timing cap in
   feedback. Input current comes from the current steering
   transistors.
2. Wire the Schmitt trigger (other TL072 half): 10k/10k
   positive feedback.
3. Connect the transistor current steering pair:
   - 2N3906 PNP: emitter to +V through resistor, collector
     to integrator summing junction
   - 2N3904 NPN: emitter to -V through resistor, collector
     to integrator summing junction
   - Bases driven from Schmitt output through resistors
4. Connect integrator output to Schmitt input.
5. Power up. Circuit should oscillate.
6. Adjust trimpot to set desired frequency.

### What to observe

- Triangle wave should be symmetric (equal up/down slopes).
- If asymmetric, current sources are not matched -- adjust
  bias resistors.
- Square wave should switch cleanly at triangle peaks.
- Frequency should be stable once the circuit warms up.

## Key Takeaways

- This is a real VCO core with proper current steering.
- Current magnitude sets frequency; Schmitt thresholds set
  amplitude. These are independent.
- Matched current sources are critical for triangle symmetry.
- Next: exponential converter (lab 08) to get V/Oct tracking
  from CV input.

## Resources

- [Falstad Circuit Simulator](https://www.falstad.com/circuit/)
  -- built-in: `e-triangle.html`
- [Moritz Klein -- VCO series](https://www.youtube.com/@MoritzKlein0)
- [Kassutronics -- VCO Part 1](https://kassu2000.blogspot.com/2015/12/vco.html)
- [Thomas Henry VCO-1](https://www.birthofasynth.com/Thomas_Henry/Pages/VCO-1.html)
