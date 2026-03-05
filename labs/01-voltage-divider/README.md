# Lab 01 -- Voltage Divider

## Goal

Understand voltage dividers: Ohm's law, node voltages, and loading
effects. These fundamentals appear throughout VCO design -- biasing
op-amps, setting reference voltages, and defining Schmitt trigger
thresholds.

## Theory

Two resistors in series between a supply voltage and ground create
a predictable voltage at their junction. The output voltage is:

```text
Vout = Vin * R2 / (R1 + R2)
```

Where R1 connects to Vin and R2 connects to ground. The midpoint
voltage depends only on the ratio of the two resistors, not their
absolute values.

### How it works

Current flows from Vin through R1 and R2 to ground. By Ohm's law,
each resistor drops a voltage proportional to its resistance. The
node between them sits at whatever voltage remains after the drop
across R1.

With equal resistors (10k/10k), the output is exactly half the
input. With a 10k/20k split, the output rises to 2/3 of Vin
because R2 carries a larger share of the total resistance.

### Loading effects

The formula above assumes no current is drawn from the output.
In practice, anything connected to the midpoint (an op-amp input,
a load resistor, a measurement probe) draws current and pulls the
voltage down. The load appears in parallel with R2, reducing the
effective lower resistance.

Loaded output voltage:

```text
R2_eff = (R2 * Rload) / (R2 + Rload)
Vout_loaded = Vin * R2_eff / (R1 + R2_eff)
```

A 10k/10k divider from 12V should give 6V. Add a 10k load and
R2_eff becomes 5k, dropping Vout to 4V. This is why low-impedance
dividers (small resistor values) are more resistant to loading,
but waste more current.

### Relevance to synth circuits

Voltage dividers appear throughout VCO designs:

- **Op-amp biasing** -- setting DC operating points at virtual
  ground or mid-supply in single-supply designs.
- **Reference voltages** -- creating stable voltage references
  from the power rails for comparators and thresholds.
- **Schmitt trigger thresholds** -- the positive feedback network
  in a Schmitt trigger (lab 05) is a voltage divider between the
  output and the non-inverting input, setting the hysteresis
  window that controls oscillation frequency.

## Simulation (Falstad)

Three Falstad circuit files are provided in the `falstad/`
directory. Import them via File > Import in the
[Falstad simulator](https://www.falstad.com/circuit/).

### Basic divider (`voltage-divider.txt`)

1. Import the file -- a 10k/10k divider from +12V with a
   voltage probe at the midpoint.
2. Observe the midpoint voltage: it should read 6.00V.
3. Right-click R2 (bottom resistor) and change its value to
   20k. The output should shift to 8.00V (12 * 20k/30k).
4. Try 4.7k for R2. Output should be ~3.84V.

### Ratio comparison (`voltage-divider-ratios.txt`)

1. Import the file -- three dividers side by side with
   different R2 values: 10k, 20k, and 4.7k.
2. Compare the three probe readings simultaneously.
3. Verify each matches the formula.

### Loaded divider (`voltage-divider-loaded.txt`)

1. Import the file -- a 10k/10k divider with a 10k load
   resistor connected to the output node.
2. The midpoint should read ~4.00V instead of 6.00V.
3. Right-click the load resistor and increase it to 100k.
   The output rises toward 6V as loading decreases.
4. Remove the load (set to a very high value like 1G) and
   confirm the output returns to 6.00V.

### Built-in example

Falstad includes a voltage divider example:
Circuits > Passive Filters > Voltage Divider. Open it and
compare with the circuits above.

## Breadboard

### Components needed

- 2x 10k ohm resistors
- 1x 4.7k ohm resistor
- 1x 22k ohm resistor
- 1x 10k ohm resistor (for load testing)
- Breadboard and jumper wires
- Multimeter
- Bench power supply set to +12V

### Procedure

1. Wire a 10k/10k divider from +12V to GND on the
   breadboard.
2. Measure the midpoint voltage with a multimeter. It should
   read close to 6.0V.
3. Replace the bottom resistor with 22k. Measure again --
   expect ~8.25V (12 * 22k/32k).
4. Replace the bottom resistor with 4.7k. Measure --
   expect ~3.84V (12 * 4.7k/14.7k).
5. Return to 10k/10k. Connect a 10k load resistor from the
   midpoint to ground. Measure the midpoint -- it should
   drop to ~4.0V.
6. Replace the load with 22k and measure again -- the
   voltage should be higher than with 10k but still below
   6V.
7. Remove the load and confirm the voltage returns to 6V.

### What to observe

- Calculated vs measured values should be within 5%
  (resistor tolerance).
- Loading always pulls the output voltage down.
- Higher-value load resistors cause less voltage drop.

## Key Takeaways

- Voltage dividers set bias points used throughout VCO
  circuits. Every op-amp stage needs DC biasing, and
  dividers are the simplest way to establish it.
- **Loading matters.** Any circuit connected to a divider
  output changes the voltage. High-impedance inputs (like
  op-amp inputs) minimize this effect.
- The Schmitt trigger threshold network in lab 05 is a
  voltage divider with positive feedback -- the same
  R1/R2 ratio math applies.
- Use low-value resistors when the output must drive a
  load. Use high-value resistors when minimizing current
  draw matters (battery-powered, CMOS logic).

## Resources

- [Falstad Circuit Simulator](https://www.falstad.com/circuit/)
- [Moritz Klein -- YouTube](https://www.youtube.com/@MoritzKlein0)
  -- VCO series covers divider biasing in context
- [Voltage Divider -- Wikipedia](https://en.wikipedia.org/wiki/Voltage_divider)
