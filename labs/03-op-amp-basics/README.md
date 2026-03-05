# Lab 03 -- Op-Amp Basics

## Goal

Understand op-amp fundamentals: inverting amplifiers,
non-inverting amplifiers, the virtual ground concept,
and voltage followers (unity-gain buffers).

## Theory

### Ideal op-amp assumptions

| Property              | Ideal value |
|-----------------------|-------------|
| Input impedance       | Infinite    |
| Output impedance      | Zero        |
| Open-loop gain        | Infinite    |
| Bandwidth             | Infinite    |

### Two golden rules (negative feedback)

1. The output adjusts so that **V+ = V-**.
2. **No current flows** into the inputs.

These two rules let you analyse any negative-feedback
op-amp circuit with basic algebra.

### Inverting amplifier

```text
        Rf
  Vin --[Rin]--+--[Rf]--+
               |         |
              -|\ Vout   |
               | >-------+
              +|/
               |
              GND
```

Gain = **-Rf / Rin**

The inverting input sits at **virtual ground**: the
feedback forces it to 0 V even though it is not
physically connected to ground.

### Non-inverting amplifier

```text
  Vin ---+
         |
        +|\ Vout
         | >-------+--
        -|/        |
         |         |
         +--[Rf]---+
         |
        [Rin]
         |
        GND
```

Gain = **1 + Rf / Rin**

### Voltage follower (unity-gain buffer)

Connect the output directly to the inverting input.
Gain = 1. Useful for impedance buffering -- high input
impedance, low output impedance.

### Power supply

Op-amps need a bipolar supply. Eurorack standard is
**+/-12 V**. Bench supplies often use +/-15 V.
The output cannot exceed the supply rails; a signal
driven beyond them **clips**.

### TL072 -- the synth-DIY workhorse

- Dual op-amp (two units in one 8-pin DIP package)
- JFET inputs (very high input impedance)
- Low noise, low offset
- Used in almost every Eurorack module

## Simulation (Falstad)

### Setup

Open [Falstad Circuit Simulator](https://falstad.com/circuit/).
Add an ideal op-amp via **Draw > Active Components > Op Amp**.
Alternatively browse **Circuits > Op-Amps** for built-in
examples.

### Exercises

#### 1 -- Inverting amplifier

Load `falstad/inverting-amp.txt`.

- Rin = 10 k, Rf = 20 k, so gain = -2.
- Apply 1 V DC input; verify output = -2 V.

#### 2 -- Non-inverting amplifier

Load `falstad/non-inverting-amp.txt`.

- Same resistors: gain = 1 + 20k/10k = 3.
- Apply 1 V DC; verify output = 3 V.

#### 3 -- Voltage follower

Load `falstad/voltage-follower.txt`.

- Output tied directly to inverting input.
- Apply 1 V DC; verify output = 1 V.

#### 4 -- Clipping

Load `falstad/inverting-clipping.txt`.

- Inverting amp with gain = -10, supply = +/-12 V.
- Apply a 2 V-peak sine; output tries to reach
  +/-20 V but clips at the rails.

## Breadboard

### Components

| Part          | Value / Type             |
|---------------|--------------------------|
| Op-amp        | TL072 (or TL071)        |
| R1 (Rin)      | 10 k                    |
| R2 (Rf)       | 20 k                    |
| R3 (optional) | 100 k                   |
| Bypass caps   | 100 nF ceramic (x2)     |
| Supply        | +/-12 V                 |

### Build steps

1. Wire +12 V to pin 8, -12 V to pin 4 of the TL072.
2. Place a **100 nF bypass cap** between each supply
   pin and ground, as close to the chip as possible.
3. Build the **inverting amplifier** (Rin = 10 k,
   Rf = 20 k) on op-amp A (pins 1-3).
   Measure gain with a DC voltage and a multimeter.
4. Rewire as a **voltage follower**: connect pin 1
   directly to pin 2. Verify unity gain.

> **Important:** always install bypass capacitors on
> the power pins. Without them the op-amp can
> oscillate or pick up noise.

## Key Takeaways

- Op-amps with negative feedback are **predictable**
  and easy to analyse with the two golden rules.
- The **inverting topology** is the basis for the
  integrator circuit in Lab 04.
- The **non-inverting topology** with positive feedback
  becomes a Schmitt trigger in Lab 05.
- **Virtual ground** is essential for understanding
  how the integrator works.

## Resources

- [TL072 datasheet (TI)](https://www.ti.com/product/TL072)
- [Moritz Klein -- Op-Amp basics (YouTube)](https://www.youtube.com/results?search_query=moritz+klein+op+amp)
- [Falstad op-amp examples](https://falstad.com/circuit/)
