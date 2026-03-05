# Lab 04 -- Integrator

## Goal

Understand the op-amp integrator: how constant current through a
capacitor creates a linear voltage ramp. This is the
ramp-generating core of every triangle and sawtooth VCO.

## Theory

Replace the feedback resistor in an inverting amplifier with a
capacitor and you get an integrator. The op-amp's virtual ground
forces a constant current through the input resistor:

```text
I = Vin / Rin
```

That current has nowhere to go except into the feedback capacitor,
which charges linearly (constant current = linear voltage change).
The output voltage over time is:

```text
V(t) = -(1 / RC) * integral(Vin) dt
```

For a DC input this simplifies to a linear ramp:

```text
Ramp rate = -Vin / (R * C)
```

### Why this is not RC charging

A bare RC circuit charges exponentially -- the voltage across the
capacitor reduces the charging current as it rises. The op-amp
integrator eliminates this problem. Virtual ground keeps the
voltage across the input resistor constant regardless of the
capacitor voltage, so current stays constant and the ramp stays
linear. This is the key insight for VCO design.

### Practical issues

- **DC drift** -- any DC offset at the input causes the output to
  ramp slowly toward the supply rail. A large resistor (1M) in
  parallel with the feedback cap bleeds off DC and limits drift.
- **Saturation** -- once the output hits the rail voltage, the
  integrator stops functioning. A reset mechanism (switch or
  Schmitt trigger feedback) is needed in practice.
- **Component choice** -- film capacitors are preferred over
  ceramic for the feedback cap. Ceramic caps have voltage-dependent
  capacitance that distorts the ramp linearity.

### Relevance to synth circuits

In a VCO, a square wave drives the integrator input. During the
positive half-cycle the output ramps down; during the negative
half-cycle it ramps up. The result is a triangle wave. The ramp
rate is proportional to the input current -- this is fundamentally
how voltage controls frequency. After exponential conversion
(lab 08), the control voltage sets the charging current and
therefore the pitch.

## Simulation (Falstad)

Three Falstad circuit files are provided in the `falstad/`
directory. Import them via File > Import in the
[Falstad simulator](https://www.falstad.com/circuit/).

### DC input (`integrator-dc.txt`)

1. Import the file -- a 10k/100nF integrator with +1V DC input
   and +/-12V supply rails.
2. Observe the output: it ramps linearly from 0V toward the
   positive rail.
3. Calculate the expected ramp rate:
   -Vin/(RC) = -1/(10k * 100n) = -1000 V/s = -1 V/ms.
4. Verify the ramp slope matches this rate on the scope.
5. The ramp hits the positive rail and saturates -- this is
   normal for an uncontrolled integrator.

### Square wave input (`integrator-square.txt`)

1. Import the file -- same integrator with a 100 Hz square wave
   input (+/-1V).
2. Observe the output: a triangle wave with the same frequency
   as the input square wave.
3. The triangle amplitude depends on RC and the input frequency.
4. Try changing R to 20k -- the ramp rate halves and the
   triangle amplitude decreases.
5. Try changing C to 220nF -- similar effect.
6. Right-click the voltage source and change the frequency to
   200 Hz. The triangle amplitude halves because the capacitor
   has less time to charge per half-cycle.

### With bleed resistor (`integrator-with-bleed.txt`)

1. Import the file -- same integrator with a 1M resistor in
   parallel with the feedback capacitor.
2. Apply the DC input and observe: the output ramps but levels
   off instead of hitting the rail. The bleed resistor limits
   the maximum output.
3. With a square wave input the triangle output is nearly
   identical to the version without bleed, because at audio
   frequencies the 1M resistor has negligible effect compared
   to the capacitor impedance.
4. This is a common practical fix for real-world integrators.

### Built-in example

Falstad includes an integrator example:
Circuits > Op-Amps > Integrator
(`circuitjs.html?startCircuit=amp-integ.txt`).
Open it and compare with the circuits above.

## Breadboard

### Components needed

- 1x TL072 dual op-amp (or TL071 single)
- 1x 10k ohm resistor
- 1x 100nF film capacitor (polyester or polypropylene)
- 1x 1M ohm resistor (for bleed version)
- Breadboard and jumper wires
- Bench power supply: +/-12V (or +/-15V)
- Function generator (square wave output)
- Oscilloscope (dual channel preferred)

### Procedure

1. Wire the TL072 as an inverting integrator: 10k from input
   to the inverting pin, 100nF from the inverting pin to the
   output. Non-inverting pin to ground. Power the op-amp with
   +/-12V.
2. Set the function generator to a 100 Hz square wave, 2Vpp
   (1V amplitude), centered at 0V.
3. Connect CH1 of the scope to the input, CH2 to the output.
4. Observe the triangle wave on CH2. It should be the integral
   of the square wave input.
5. Measure the ramp rate on the scope. Expected:
   Vin/(RC) = 1V / (10k * 100nF) = 1000 V/s = 1V/ms.
6. Over one half-period (5ms at 100Hz) the ramp should swing
   about 5V peak-to-peak.
7. Change the input frequency to 200 Hz. The triangle
   amplitude should halve (less time per ramp).
8. Add a 1M resistor in parallel with the 100nF cap. At
   100 Hz you should see minimal difference. With a very low
   frequency (1-5 Hz) the bleed effect becomes visible.

### What to observe

- Triangle output should have straight ramp segments, not
  curved (exponential). This confirms the integrator works.
- Calculated vs measured ramp rates should be within 10%.
- If the output drifts to the rail with no input, the op-amp
  has input offset voltage -- the bleed resistor fixes this.

## Key Takeaways

- The integrator converts constant current into a linear
  voltage ramp -- the core mechanism of VCO waveform
  generation.
- **Square wave in, triangle wave out.** This is exactly what
  happens inside a triangle-core VCO.
- In a VCO, the Schmitt trigger (lab 05) provides the
  switching square wave that reverses the ramp direction,
  creating continuous oscillation.
- Ramp rate is proportional to input current. This is how
  voltage controls frequency: more current means faster ramp,
  higher frequency. After exponential conversion (lab 08),
  this relationship becomes volts-per-octave.
- Film capacitors give better linearity than ceramic for
  integrator feedback.

## Resources

- [Falstad Circuit Simulator](https://www.falstad.com/circuit/)
  -- built-in integrator: `amp-integ.txt`
- [Moritz Klein -- VCO Part 1](https://www.youtube.com/watch?v=QBatvo8bCa4)
  -- integrator and triangle core explanation
- [Op Amp Applications Handbook -- ADI](https://www.analog.com/en/education/education-library/op-amp-applications-handbook.html)
  -- chapter on integrators and differentiators
