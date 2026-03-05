# Lab 10 -- Full Discrete VCO

## Goal

Combine labs 07-09 into a complete discrete triangle-core VCO
with V/Oct tracking, triangle, square, sine, and PWM outputs.

## Theory

### Signal chain

```text
CV input --> expo converter (lab 08)
         --> current source
         --> integrator core (lab 07)
         --> triangle output
             |
             +--> Schmitt trigger --> square output
             |
             +--> diode waveshaper (lab 09) --> sine output
             |
             +--> comparator + threshold --> PWM output
```

### Tuning controls

- **Coarse frequency**: sets the base oscillation frequency
  (typically via a panel knob controlling the expo converter
  reference current).
- **Fine tune**: small range adjustment around the coarse
  setting (usually +/-1 semitone).
- **V/Oct trim**: precision trimpot that scales the CV input
  so that exactly 1V produces exactly one octave change.
- **HF tracking trim**: compensates for expo converter errors
  that worsen at high frequencies (parasitic capacitance,
  transistor bandwidth limits).

### Power supply considerations

- Every op-amp needs 100nF ceramic bypass caps on both power
  pins, placed as close to the IC as possible.
- Use star grounding or a ground plane to minimize noise.
- Keep the expo converter transistors away from heat sources.
- A separate regulated +/-12V for the expo converter section
  improves stability.

## Simulation (LTspice)

Open `ltspice/full-discrete-vco.asc` in LTspice.

### V/Oct verification

1. The simulation sweeps CV from 0V to 5V in 1V steps.
2. Run the transient simulation.
3. Measure frequency at each CV voltage using cursors or
   the `.meas` directives.
4. Plot frequency vs CV on a log-linear scale -- should be
   a straight line.
5. Each 1V step should approximately double the frequency.

### Output waveforms

1. Probe the triangle, square, sine, and PWM outputs.
2. Triangle should have linear ramps and symmetric shape.
3. Square should have fast, clean transitions.
4. Sine should show rounded peaks (check THD with `.four`).
5. PWM width should vary with the threshold voltage.

### Temperature sweep

1. Enable `.step temp 0 50 5` to check stability.
2. With tempco resistor: frequency should remain stable
   across the temperature range.
3. Without tempco: observe the drift magnitude.

## Breadboard

### Components needed

- 2x TL072 dual op-amp (4 op-amp sections total)
- 2x 2N3904 NPN (expo pair, hand-matched)
- 2x 2N3904 NPN (waveshaper differential pair)
- 1x 2N3906 PNP (current steering)
- 4-8x 1N4148 diodes (waveshaper)
- 1x 1k PTC tempco resistor (3300 ppm/K)
- 1x 10nF film or C0G capacitor (timing)
- Assorted resistors: 1k, 4.7k, 10k, 47k, 100k (1% metal
  film preferred)
- 3x 10-turn 10k trimpots (coarse, V/Oct, HF trim)
- 1x 10k potentiometer (PWM width)
- 100nF ceramic caps for power bypassing (x4)
- Bench power supply: +/-12V
- Oscilloscope (dual channel minimum)
- Multimeter

### Build strategy

Do NOT build everything at once. Build and test section by
section:

1. **Core oscillator** (lab 07): verify triangle and square
   outputs, stable oscillation.
2. **Add expo converter** (lab 08): connect CV input, verify
   frequency changes with voltage. Rough V/Oct tracking.
3. **Add waveshaper** (lab 09): verify sine output quality.
4. **Add PWM comparator**: verify variable pulse width.
5. **Calibrate**: adjust V/Oct trim, HF trim, coarse
   frequency range.

### Calibration procedure

1. Set coarse frequency to ~261 Hz (middle C, C4).
2. Apply 0V CV. Record the frequency.
3. Apply 1V CV. Adjust V/Oct trim until frequency is exactly
   2x the 0V frequency.
4. Apply 2V, 3V, 4V CV. Verify doubling at each step.
5. If high octaves track poorly, adjust HF trim.
6. Repeat steps 2-5 until tracking is acceptable across
   3-4 octaves.

### What to observe

- Perfect tracking across 5+ octaves is unlikely on a
  breadboard. 3-4 octaves is a good result.
- Temperature sensitivity is real -- let the circuit warm up
  for 10 minutes before calibrating.
- Touch the expo transistors and watch the frequency shift.
  This demonstrates why thermal coupling matters.

## Key Takeaways

- You have built a complete analog VCO from discrete
  components. Four waveform outputs, voltage control,
  and V/Oct tracking.
- Expect imperfect tracking -- this is why calibration,
  component matching, and thermal management matter.
- The CEM3340 (lab 11) integrates all of this on a single
  chip with laser-trimmed precision. Having built it
  yourself, you understand what the chip does internally.
- Breadboard parasitics (stray capacitance, poor grounding)
  limit performance. A PCB would improve everything.

## Resources

- [Moritz Klein -- Full VCO Build](https://www.youtube.com/@MoritzKlein0)
- [Kassutronics -- Discrete VCO](https://kassu2000.blogspot.com/2015/12/vco.html)
- [Thomas Henry VCO-1](https://www.birthofasynth.com/Thomas_Henry/Pages/VCO-1.html)
- [Erica Synths -- EDU VCO Manual](https://www.ericasynths.lv/shop/diy-kits-1/edu-diy-vco/)
