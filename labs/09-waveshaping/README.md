# Lab 09 -- Waveshaping

## Goal

Convert the triangle wave from the VCO core into a sine wave,
and generate PWM (pulse width modulation) from the triangle
output. These add musically useful waveforms to your VCO.

## Theory

### Triangle to sine

The triangle wave's sharp peaks need to be rounded off to
approximate a sine wave. Two common approaches:

**Diode waveshaper**: A network of series/parallel diodes with
resistors progressively clips the triangle peaks. Each diode
pair activates at a different voltage level, creating a
piecewise-linear approximation of a sine curve. With 4-6
diode segments, distortion below 1% THD is achievable.

The Falstad built-in example `e-sinediode.html` demonstrates
this technique.

**Differential pair**: Feed the triangle into a BJT
differential pair. The tanh transfer function of the diff pair
naturally rounds the peaks. The amplitude must be scaled so
the triangle peaks enter the nonlinear region of the tanh
curve. Simpler circuit than the diode approach but harder to
optimize for low distortion.

### Pulse width modulation (PWM)

Compare the triangle wave with a DC threshold using a
comparator (or Schmitt trigger without hysteresis). When the
triangle exceeds the threshold, output goes HIGH; when below,
output goes LOW.

- Threshold at 0V: 50% duty cycle = square wave
- Threshold shifted positive: narrow positive pulses
- Threshold shifted negative: wide positive pulses

CV-controllable PWM is a signature VCO feature. Sweeping the
pulse width creates the classic "phasing" or "chorus" effect
heard in many classic synthesizers.

## Simulation

### Falstad

Import files from `falstad/` via File > Import in the
[Falstad simulator](https://www.falstad.com/circuit/).

#### Diode waveshaper (`diode-waveshaper.txt`)

1. Triangle wave input fed through a diode clipping network.
2. Compare input triangle with shaped output on the scope.
3. Adjust diode network resistor values to tune the shape.
4. More diode segments = closer sine approximation.

Also see the Falstad built-in: `e-sinediode.html`.

#### PWM generator (`pwm-generator.txt`)

1. Triangle wave input to a comparator. DC threshold sets
   the pulse width.
2. Adjust the threshold voltage and watch the duty cycle
   change.
3. At 0V threshold: 50% duty cycle (square wave).
4. Positive threshold: narrow pulses. Negative: wide pulses.

### LTspice (`ltspice/waveshaper.asc`)

Open in LTspice and run the simulation.

1. Differential pair (2N3904) triangle-to-sine converter.
2. Triangle input via PULSE source.
3. `.tran` simulation shows the shaped output.
4. `.four` directive measures THD.
5. Adjust the input amplitude scaling to optimize the sine
   shape.

## Breadboard

### Components needed

For diode waveshaper:

- 4-8x 1N4148 signal diodes
- Resistor network (values from simulation)
- 1x TL072 op-amp (output buffer)

For PWM:

- 1x TL072 (comparator mode)
- 1x 10k potentiometer (threshold adjust)

Common:

- Bench power supply: +/-12V
- Oscilloscope
- Triangle wave source (lab 07 VCO or function generator)

### Procedure

#### Diode waveshaper

1. Start with a simple 2-diode clipper: two antiparallel
   1N4148s with series resistors.
2. Feed triangle wave from the VCO (or function generator).
3. Observe output on scope -- peaks should be rounded.
4. Add more diode stages for better sine approximation.
5. Buffer the output with a TL072 voltage follower.

#### PWM generator

1. Wire a TL072 as open-loop comparator: triangle to one
   input, potentiometer voltage divider to the other.
2. Turn the pot to vary pulse width from nearly 0% to 100%.
3. Observe on scope: clean rectangular pulses with variable
   width.

### What to observe

- Sine output should look visually smooth on the scope.
- PWM should have clean, fast transitions (op-amp slew rate
  is the limiting factor).
- Rotating the PWM pot should sweep smoothly from narrow
  to wide pulses.

## Key Takeaways

- Waveshaping adds musically useful outputs (sine, PWM) to
  the basic triangle-core VCO.
- Diode shaping is simple but requires careful resistor
  tuning for low distortion.
- Differential pair shaping is elegant but sensitive to input
  amplitude.
- PWM creates the classic "phasing" sound when pulse width is
  modulated by an LFO or envelope.
- These are the final building blocks before the complete
  discrete VCO (lab 10).

## Resources

- [Falstad Circuit Simulator](https://www.falstad.com/circuit/)
  -- built-in: `e-sinediode.html`
- [Moritz Klein -- VCO Part 4](https://www.youtube.com/@MoritzKlein0)
  -- waveshaping and PWM
- [Kassutronics -- VCO Part 2](https://kassu2000.blogspot.com/2016/02/vco-part-2.html)
  -- waveshaper design details
