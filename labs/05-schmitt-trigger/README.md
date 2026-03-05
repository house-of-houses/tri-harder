# Lab 05 -- Schmitt Trigger

## Goal

Understand positive feedback, hysteresis, and the Schmitt trigger
comparator. This is the switching element that tells the VCO
integrator (lab 04) when to reverse direction -- essential for
generating a continuous triangle wave.

## Theory

### Positive feedback

A Schmitt trigger is the opposite of the negative feedback
circuits from lab 03. Instead of feeding the output back to the
inverting (-) input, the output feeds back to the non-inverting
(+) input. This creates regenerative behavior: once the output
starts to change, the feedback reinforces the change until the
output slams to one of the supply rails.

### Two thresholds and hysteresis

The positive feedback network is a voltage divider between the
op-amp output and the input signal, connected to the (+) input.
This creates two distinct switching thresholds:

- **V_high** -- input voltage that triggers a switch to -Vsat
- **V_low** -- input voltage that triggers a switch to +Vsat

The gap between these thresholds is the **hysteresis band**.

### Threshold calculation

With equal feedback resistors R1 = R2 (both 10k), the voltage
at the (+) input is the midpoint between the output and the
input. The thresholds work out to:

```text
V_high = +Vsat / 2    (about +5.5V with +/-12V supply)
V_low  = -Vsat / 2    (about -5.5V with +/-12V supply)
Hysteresis = V_high - V_low = Vsat (about 11V)
```

With unequal resistors, the thresholds shift. A larger feedback
resistor (from output) relative to the input resistor narrows
the hysteresis band. For R1 (input to +) = 47k and
R2 (output to +) = 10k:

```text
V_high = +Vsat * R1 / (R1 + R2) = +Vsat * 47/57 ~ +/-9.9V
```

This gives a much narrower hysteresis window.

### Snap action

The switching is regenerative. When the input crosses a
threshold, the output begins to move. This movement feeds back
to the (+) input, pushing it further past the threshold, which
drives the output harder. The result is a near-instantaneous
snap from one rail to the other -- regardless of how slowly the
input signal is moving.

### Noise immunity

A simple comparator (no hysteresis) switches at a single
threshold. Any noise near that threshold causes rapid,
uncontrolled toggling (chatter). The Schmitt trigger's two
separated thresholds prevent this: once the output has switched,
the input must travel the full hysteresis band before it can
switch back.

### Relevance to VCO design

In a triangle-core VCO, the Schmitt trigger output controls
the integrator direction. When the triangle wave reaches the
upper threshold, the Schmitt trigger snaps to -Vsat, causing
the integrator to ramp downward. When the triangle reaches the
lower threshold, it snaps to +Vsat and the integrator ramps
back up. The threshold voltages directly determine the triangle
wave amplitude.

## Simulation (Falstad)

Three Falstad circuit files are provided in `falstad/`.
Open [Falstad](https://www.falstad.com/circuit/) and use
File > Import to load each one.

### 1. Basic Schmitt trigger (schmitt-trigger.txt)

- Op-amp with 10k/10k positive feedback and slow sine input
- Watch how the output snaps between +Vsat and -Vsat
- The scope shows input (green) and output (yellow)
- Note the output transitions happen at different input
  voltages on the rising vs falling edges -- this is
  hysteresis
- Right-click a feedback resistor and change its value to
  see how thresholds shift

### 2. Narrow hysteresis (schmitt-hysteresis.txt)

- Same topology but with 47k (input) and 10k (feedback)
  resistors for a narrower hysteresis band
- Compare the threshold voltages with the basic circuit
- The output still snaps cleanly but triggers closer to 0V
- Useful when you want smaller triangle wave amplitude in
  the VCO

### 3. Noisy input (schmitt-noisy-input.txt)

- Schmitt trigger driven by a sine wave with added noise
- Despite the noisy input, the output switches cleanly
- A plain comparator would chatter at the crossings -- the
  hysteresis band rejects the noise
- Try reducing the feedback resistor ratio to narrow
  hysteresis and see when noise starts causing false triggers

### Built-in example

Falstad includes a Schmitt trigger demo:
`Circuits > Op-Amps > Schmitt Trigger`. Open it and compare
with the circuits above.

## Breadboard

### Components

- 1x TL072 dual op-amp (or TL071 single)
- 2x 10k ohm resistors (positive feedback network)
- 2x 47k ohm resistors (for narrow hysteresis variant)
- Breadboard and jumper wires
- +/-12V dual power supply
- Function generator
- Oscilloscope (2 channels)

### Procedure

1. Wire the TL072 with +12V on pin 8, -12V on pin 4.
2. Connect the inverting (-) input directly to ground.
3. Connect a 10k resistor from the input signal to the
   non-inverting (+) input.
4. Connect a 10k resistor from the output back to the
   non-inverting (+) input. This is the positive feedback.
5. Set the function generator to a triangle wave, ~100 Hz,
   10Vpp amplitude.
6. Connect scope channel 1 to the input, channel 2 to the
   output.
7. Observe the square wave output. Note the input voltage
   levels where switching occurs -- these are V_high and
   V_low.
8. Measure both thresholds with the scope cursors. They
   should be approximately +/-5.5V.
9. Replace the 10k feedback resistor (output to +) with
   47k. The input resistor stays 10k.
10. Observe how the thresholds move closer to 0V -- the
    hysteresis band narrows.
11. Try the reverse: 10k feedback, 47k input resistor.
    The hysteresis band widens.

### What to observe

- Output is always at +Vsat or -Vsat -- never in between
- Switching is instantaneous regardless of input slew rate
- Threshold voltages match the resistor ratio predictions
- With narrow hysteresis, the circuit is more sensitive to
  noise on the input

## Key Takeaways

- The Schmitt trigger provides the switching signal that
  reverses the integrator in a triangle-core VCO.
- **Wider hysteresis = larger triangle amplitude.** The
  threshold voltages directly set how far the integrator
  ramps before reversing.
- The feedback resistor ratio is the single design parameter
  that controls the hysteresis width and therefore the
  triangle wave amplitude.
- Combined with the integrator from lab 04, the Schmitt
  trigger forms a complete relaxation oscillator -- which
  is exactly what lab 06 builds.

## Resources

- [Falstad Circuit Simulator](https://www.falstad.com/circuit/)
  -- Schmitt Trigger example under Op-Amps menu
- [Moritz Klein -- YouTube](https://www.youtube.com/@MoritzKlein0)
  -- VCO series covers Schmitt trigger role in oscillators
- [Schmitt Trigger -- Wikipedia](https://en.wikipedia.org/wiki/Schmitt_trigger)
- [Op-Amp Schmitt Trigger -- Electronics Tutorials](https://www.electronics-tutorials.ws/opamp/op-amp-schmitt-trigger.html)
