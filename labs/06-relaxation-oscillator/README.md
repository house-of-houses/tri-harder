# Lab 06 -- Relaxation Oscillator

## Goal

Combine the integrator (lab 04) and Schmitt trigger (lab 05) into
a self-oscillating circuit. This is the fundamental VCO topology.

## Theory

Connect the Schmitt trigger output back to the integrator input
and the circuit oscillates on its own:

1. Schmitt outputs +Vsat --> integrator ramps down
2. Output crosses lower Schmitt threshold --> Schmitt flips to
   -Vsat
3. Integrator ramps up
4. Output crosses upper threshold --> Schmitt flips back to +Vsat
5. Repeat

The result: a **triangle wave** at the integrator output and a
**square wave** at the Schmitt trigger output.

### Frequency

For symmetric oscillation with equal Schmitt thresholds:

```text
f = 1 / (4 * R * C)
```

Where R and C are the integrator components. The factor of 4
comes from two ramp directions times two threshold crossings
per cycle.

### From fixed oscillator to VCO

This relaxation oscillator runs at a fixed frequency set by R
and C. To make it voltage-controlled, replace the fixed input
resistor with a voltage-controlled current source. The
exponential converter (lab 08) provides this: CV voltage sets
current, current sets ramp rate, ramp rate sets frequency.

### Why the TL072 is perfect

The TL072 is a dual op-amp in one 8-pin package. One half
becomes the integrator, the other the Schmitt trigger. This
is why the TL072 is the most common IC in synth DIY.

## Simulation (Falstad)

Three circuit files are provided in `falstad/`. Import via
File > Import in the
[Falstad simulator](https://www.falstad.com/circuit/).

### Two-op-amp oscillator (`relaxation-oscillator.txt`)

1. Import the file -- integrator (10k, 100nF) plus Schmitt
   trigger (10k/10k positive feedback) connected in a loop.
2. Oscillation should start immediately.
3. Observe: triangle wave at integrator output, square wave at
   Schmitt output.
4. Expected frequency: 1/(4 x 10k x 100nF) = 250 Hz.
5. Right-click the timing cap and change to 47nF -- frequency
   roughly doubles.
6. Right-click a Schmitt feedback resistor and change to 22k --
   the triangle amplitude changes.

### Single op-amp version (`single-opamp-relaxosc.txt`)

1. Import the file -- classic RC + comparator with hysteresis.
2. The capacitor charges through the resistor (exponentially,
   not linearly) until it crosses the Schmitt threshold.
3. Output is a square wave; the capacitor voltage is an
   exponential sawtooth, not a clean triangle.
4. Simpler but less useful for VCO design due to the nonlinear
   waveform.

### Variable frequency (`frequency-control.txt`)

1. Import the file -- two-op-amp oscillator with the integrator
   input resistor replaced by a potentiometer (variable
   resistor).
2. Adjust the potentiometer value and watch the frequency
   change.
3. This demonstrates the principle behind voltage control:
   changing the charging current changes the frequency.

### Built-in examples

Falstad includes related examples:

- Circuits > Op-Amps > Triangle Wave
  (`e-triangle.html`)
- Circuits > Op-Amps > Relaxation Oscillator
  (`e-relaxosc.html`)

Open them and compare with the circuits above.

## Breadboard

### Components needed

- 1x TL072 dual op-amp (uses both halves)
- 3x 10k ohm resistors
- 1x 100nF film capacitor
- 1x 10k trimpot (optional, for frequency control)
- Breadboard and jumper wires
- Bench power supply: +/-12V
- Oscilloscope (dual channel)

### Procedure

1. Wire the TL072: one half as integrator (pin 2 = inverting
   input, pin 1 = output, 10k input resistor, 100nF feedback
   cap). Other half as Schmitt trigger (pin 5 = non-inverting
   input, pin 7 = output, 10k/10k positive feedback divider
   from output to non-inverting input).
2. Connect Schmitt output (pin 7) to integrator input resistor.
3. Connect integrator output (pin 1) to Schmitt inverting
   input (pin 6).
4. Power with +/-12V. Add 100nF bypass caps on power pins.
5. The circuit should oscillate immediately.
6. Probe pin 1 (triangle output) on CH1 and pin 7 (square
   output) on CH2.
7. Measure frequency. Should be near 250 Hz with 10k/100nF.
8. Replace the 100nF cap with 10nF -- frequency jumps to
   ~2.5 kHz.

### What to observe

- Triangle wave should have straight ramp segments (linear).
- Square wave transitions should be fast and clean.
- Both outputs should have the same frequency.
- Changing the cap value scales frequency proportionally.

## Key Takeaways

- You have built the core of a VCO. Triangle and square wave
  outputs come naturally from the integrator-Schmitt topology.
- Frequency depends on the RC time constant and the Schmitt
  trigger thresholds.
- To turn this into a real VCO: replace the fixed input
  resistor with a voltage-controlled current source (expo
  converter, lab 08).
- The TL072 dual op-amp provides both functions in one chip --
  this is why it dominates synth DIY.

## Resources

- [Falstad Circuit Simulator](https://www.falstad.com/circuit/)
  -- built-in: `e-triangle.html`, `e-relaxosc.html`
- [Moritz Klein -- VCO Part 1](https://www.youtube.com/@MoritzKlein0)
  -- oscillator core explanation
- [Electronics Tutorials -- Relaxation Oscillator](https://www.electronics-tutorials.ws/waveforms/generators.html)
