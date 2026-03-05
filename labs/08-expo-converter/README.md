# Lab 08 -- Exponential Converter

## Goal

Understand the exponential converter: how it converts a linear
CV voltage (1V/octave) into an exponential current to drive
the VCO core. This is what makes a VCO track musically across
octaves.

## Theory

### The BJT exponential relationship

A bipolar transistor's collector current depends exponentially
on its base-emitter voltage:

```text
Ic = Is * e^(Vbe / Vt)
```

Where `Vt = kT/q` is the thermal voltage (~26mV at 25C). This
exponential relationship is exactly what V/Oct needs: each 1V
increase in CV should double the frequency.

### Matched transistor pair

Two transistors with identical Is (saturation current) form the
expo converter core. One converts CV voltage to current, the
other provides a reference. The ratio of their collector
currents:

```text
Ic1 / Ic2 = e^(deltaVbe / Vt)
```

When deltaVbe is set by the CV input through a precision
resistor, the output current tracks exponentially with CV.

### Why matching matters

Any difference in Is between the two transistors causes a
constant offset in the current ratio. This shows up as
tracking errors across octaves. Solutions:

- **THAT340/300 series**: laser-trimmed matched pairs on a
  monolithic die (500uV offset specification).
- **Hand-matched 2N3904s**: measure Vbe at a fixed current,
  select pairs within 1mV. Good enough for breadboarding.
- **Thermal coupling**: both transistors must be at the same
  temperature. Tape, heatshrink, or thermal paste them
  together.

### Temperature compensation

The Vt term is proportional to absolute temperature. Without
compensation, the VCO drifts ~0.3%/C (~5 cents/C). A tempco
resistor (3300 ppm/K PTC) in the CV scaling network cancels
this drift:

```text
R(T) = R0 * (1 + 3353e-6 * (T - 25))
```

The value 3353 ppm/K equals 1/T_absolute (1/298.15K at 25C),
matching the Vt temperature coefficient exactly.

## Simulation (LTspice)

Open `ltspice/expo-converter.asc` in LTspice.

### DC sweep

1. The circuit sweeps CV input from 0V to 5V (5 octaves).
2. Probe the output collector current.
3. View on a log scale: a straight line confirms exponential
   behavior.
4. Verify: current should double for each 1V increase.

### Temperature analysis

1. Run the `.step temp 0 50 5` directive.
2. Without tempco: the current-vs-voltage curves shift with
   temperature.
3. With tempco resistor (tc1=3.353e-3): the curves should
   overlap, confirming compensation.

### Integration with VCO core

1. Connect expo converter output to the integrator current
   input from lab 07.
2. Sweep CV and measure oscillation frequency.
3. Plot frequency on log scale vs CV on linear scale -- should
   be a straight line for proper V/Oct tracking.

## Breadboard

### Components needed

- 2x 2N3904 NPN transistors (select a matched pair)
- 1x TL072 dual op-amp
- 1x 100k ohm resistor (CV scaling)
- 1x 1k ohm PTC tempco resistor (3300 ppm/K) -- or use a
  regular 1k and accept temperature drift for now
- Various precision resistors (1% or better)
- Bench power supply: +/-12V
- Multimeter (for transistor matching)
- Oscilloscope

### Transistor matching procedure

1. Set up a constant current source: 10k resistor from +12V
   to collector, emitter to ground, base to a ~0.7V bias.
2. Measure Vbe with multimeter (4.5 digit preferred).
3. Test 10+ transistors at the same current (~1mA).
4. Select pairs within 1mV of each other.
5. Mark the matched pair and tape them together for thermal
   coupling.

### Circuit build

1. Wire the matched pair: emitters tied together through the
   tempco resistor.
2. One base receives the CV input (scaled by the 100k
   resistor), the other receives a reference voltage.
3. Op-amp current mirror on the collector converts the current
   to a voltage for driving the integrator.
4. Measure collector current vs CV voltage with multimeter.
5. Should see roughly 2x current increase per volt.

### What to observe

- Perfect V/Oct tracking on a breadboard is very difficult.
- Focus on understanding the concept, not achieving precision.
- Temperature drift is real -- touch the transistors and watch
  the frequency shift.

## Key Takeaways

- The expo converter is the most temperamental part of a VCO.
  Temperature stability requires matched transistors plus a
  tempco resistor.
- This is why CEM3340/AS3340 chips exist: they integrate a
  laser-trimmed expo converter on-die, solving the hardest
  problem in VCO design.
- For discrete VCOs, expect significant calibration effort.
- V/Oct precision depends on: transistor matching, thermal
  coupling, tempco accuracy, and CV input resistor tolerance.

## Resources

- [Rene Schmitz -- Expo Tutorial](https://www.schmitzbits.de/expo_tutorial/)
  -- definitive reference for expo converter theory
- [Moritz Klein -- VCO Part 2](https://www.youtube.com/@MoritzKlein0)
  -- exponential converter explanation
- [Lanterman ECE4450](https://www.youtube.com/@Lantertronics)
  -- academic treatment of expo conversion
- [THAT340 Datasheet](https://thatcorp.com/datashts/THAT_300-Series_Datasheet.pdf)
  -- matched transistor pair specifications
- [Ian Fritz -- Tempco Equations](http://ijfritz.byethost4.com/sy_cir7.htm)
