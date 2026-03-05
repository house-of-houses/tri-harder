# Lab 02 -- RC Circuits

## Goal

Understand RC charge/discharge behavior, time constants,
and frequency response. These concepts are foundational for
the integrator (lab 04) and ultimately the VCO oscillator core.

## Theory

### RC time constant

The time constant of an RC circuit is:

```text
tau = R x C
```

For R = 10k and C = 100nF: tau = 1ms.

### Charge and discharge

A capacitor charges through a resistor exponentially:

```text
V(t) = Vmax * (1 - e^(-t/tau))    (charging)
V(t) = Vmax * e^(-t/tau)           (discharging)
```

Key milestones during charging:

| Time   | Voltage reached |
|--------|-----------------|
| 1 tau  | 63.2%           |
| 2 tau  | 86.5%           |
| 3 tau  | 95.0%           |
| 5 tau  | 99.3%           |

### Cutoff frequency

An RC circuit acts as a low-pass filter with cutoff:

```text
f_c = 1 / (2 * pi * R * C)
```

For 10k + 100nF: f_c = ~159 Hz. Signals below f_c pass
through mostly unchanged; signals above f_c get attenuated
at -20 dB/decade.

### Relevance to VCO design

- Integrators (lab 04) use RC to generate voltage ramps
- The RC time constant sets the oscillation frequency
- Key insight: with a resistor, capacitor charging is
  exponential. With constant current (op-amp integrator),
  it becomes **linear** -- that is what makes a clean
  triangle/sawtooth wave possible

## Simulation (Falstad)

Three simulation files are provided in `falstad/`.
Open [Falstad](https://www.falstad.com/circuit/) and use
File > Import to load each one.

### 1. Charge/discharge (rc-charge-discharge.txt)

- 10k resistor + 100nF capacitor driven by 1 Hz square wave
- Watch the exponential charge and discharge curves
- Measure time from 0% to 63.2% -- should be ~1ms (= tau)
- Try changing R or C and observe the effect on tau

### 2. Low-pass filter (rc-low-pass.txt)

- Same RC values with a sine wave input at ~1 kHz
- Scope shows input vs. output (attenuated + phase-shifted)
- Try sweeping the frequency: below 159 Hz output tracks
  input; above 159 Hz it rolls off

### 3. Multiple time constants (rc-time-constants.txt)

- Three RC circuits side by side: 1ms, 10ms, 100ms
- All driven by the same square wave
- Compare how fast each capacitor charges and discharges
- Visually confirms that larger tau = slower response

### Also try

Falstad built-in: Circuits > Passive Filters for additional
low-pass and high-pass RC filter examples.

## Breadboard

### Components

- 1x 10k resistor
- 1x 100nF ceramic capacitor

### Setup

1. Connect function generator output to one end of the 10k
   resistor (or use bench supply switched manually)
2. Connect other end of resistor to one leg of the capacitor
3. Connect other leg of capacitor to ground
4. Probe the capacitor voltage (resistor-capacitor junction)
   with oscilloscope channel 1
5. Probe the input signal with oscilloscope channel 2

### What to measure

- Set function generator to square wave, ~100 Hz
- Measure the time from 0% to 63.2% on the scope -- this
  is tau and should read approximately 1ms
- Switch to sine wave and sweep frequency to observe the
  low-pass filter behavior
- Replace the 100nF cap with a 1uF cap (tau = 10ms) and
  repeat -- the charge/discharge should be visibly slower

## Key Takeaways

1. **RC = foundation of integrators.** Lab 04 builds on this
   directly -- the integrator is just an RC circuit with an
   op-amp enforcing constant current.
2. **Time constant sets frequency.** In a VCO, the RC time
   constant (or its equivalent with a current source +
   capacitor) determines the oscillation frequency.
3. **Exponential vs. linear charging.** Through a resistor,
   capacitor voltage is exponential. Through a constant
   current source (op-amp integrator), it becomes linear.
   This is the key insight for building a triangle-core VCO.

## Resources

- [Falstad Circuit Simulator](https://www.falstad.com/circuit/)
- [RC Circuit Tutorial -- Electronics Tutorials](https://www.electronics-tutorials.ws/rc/rc_1.html)
- [RC Time Constant -- All About Circuits](https://www.allaboutcircuits.com/textbook/direct-current/chpt-16/voltage-and-current-calculations/)
- [Low-Pass Filter -- SparkFun](https://learn.sparkfun.com/tutorials/capacitors/application-examples)
