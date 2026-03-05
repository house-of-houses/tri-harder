# Lab 11 -- CEM3340 VCO

## Goal

Build a VCO using the CEM3340/AS3340 IC. Understand what the
chip integrates internally and how classic synthesizers used it
with different output stage designs.

## Theory

### What the chip does

The CEM3340 (Curtis Electromusic, 1981) integrates the entire
VCO core on one chip:

- **Precision expo converter** with on-chip temperature
  compensation (no external tempco needed)
- **Sawtooth-core oscillator** with capacitor-discharge reset
- **Triangle converter** (sawtooth to triangle)
- **Pulse converter** with PWM input
- **Internal voltage reference**

Modern equivalents: AS3340 (Alfa), V3340 (CoolAudio). All are
pin-compatible.

### Key differences from discrete

The CEM3340 uses a **sawtooth core**, not triangle. A ramp
capacitor charges linearly until a threshold is reached, then
discharges rapidly through an internal transistor switch. This
is different from the bidirectional triangle core in labs 07-10.

Advantages over discrete:

- 10+ octave tracking (vs 3-4 on a breadboard)
- Much faster calibration
- No external tempco or matched transistors needed
- Proven in hundreds of classic synth designs

### Output levels (important!)

The three outputs have **different voltage ranges**:

| Output   | Range      | Eurorack target |
|----------|------------|-----------------|
| Sawtooth | 0 to +10V  | +/-5V (10Vpp)   |
| Triangle | 0 to +5V   | +/-5V (10Vpp)   |
| Pulse    | 0 to +13.7V| +/-5V (10Vpp)   |

Output buffering and level shifting is required to match
Eurorack standards. This is where most design variation
between classic synths occurs.

### Classic implementations

Every major polysynth used the CEM3340 differently:

- **Prophet 5 Rev.3**: AC-coupled saw output, simple
  pulse buffer
- **OB-Xa**: DC-coupled outputs, different gain staging
- **Jupiter-6**: elaborate output conditioning with
  multiple trims
- **Memorymoog**: unique waveshaping additions

See the Electric Druid article for detailed comparisons.

### Hard sync

The CEM3340 has a dedicated sync input. When a sync pulse
arrives, the oscillator resets its ramp regardless of where
it is in the cycle. This creates the characteristic "sync
sweep" sound when the synced oscillator's frequency is swept.

## Simulation (LTspice)

Open `ltspice/cem3340-output-stage.asc` in LTspice.

This simulation focuses on **output buffering** rather than
the chip internals (which are not publicly modeled):

1. Behavioral voltage sources simulate the three CEM3340
   outputs at their native levels.
2. Output buffer circuits scale and shift each waveform to
   +/-5V (Eurorack standard 10Vpp).
3. Run the transient simulation and probe each buffered
   output.
4. Verify all outputs are centered at 0V with 10Vpp swing.
5. Compare different buffer topologies (inverting vs
   non-inverting, AC vs DC coupling).

## Breadboard

### Components needed

- 1x AS3340 or V3340 (DIP-16 package)
- 2x TL072 dual op-amp (output buffers)
- 1x 1nF polystyrene or C0G capacitor (timing)
- Precision resistors (1% metal film): 1k, 10k, 15k,
  47k, 100k, 1M
- 2x 10-turn trimpots: 100k (V/Oct), 10k (HF trim)
- 1x 16-pin DIP socket
- 100nF ceramic bypass caps (x4)
- Bench power supply: +/-12V
- Oscilloscope
- Multimeter

### Procedure

1. Socket the AS3340 -- never solder ICs directly.
2. Wire power: pin 16 = +V, pin 3 = -V, pin 1 = ground.
   Add 100nF bypass caps on both supply pins.
3. Timing capacitor: 1nF between pin 11 and ground.
4. CV input: connect through the V/Oct scaling network
   to pin 15 (exponential CV input).
5. Hard sync: pin 6 (leave unconnected or tie to ground
   if not using sync).
6. PWM: pin 5 (connect a pot for manual PWM, or tie to
   a fixed voltage for 50% duty).

### Output buffering

1. Sawtooth (pin 10): inverting amplifier to shift 0-10V
   down to +/-5V. Gain = -1, offset via summing resistor.
2. Triangle (pin 9): amplify 0-5V to +/-5V. Gain = -2,
   offset via summing.
3. Pulse (pin 4): voltage divider + buffer to bring
   0-13.7V down to +/-5V.

### Calibration

1. Apply 0V CV. Adjust coarse frequency to ~261 Hz (C4).
2. Apply 1V CV. Adjust V/Oct trim until frequency is
   exactly 523 Hz (C5, one octave up).
3. Check 2V (1046 Hz), 3V (2093 Hz), 4V (4186 Hz).
4. If high octaves drift flat, increase HF trim slightly.
5. The AS3340 should track well over 8+ octaves with
   proper trimming.

### What to observe

- Calibration is dramatically easier than the discrete VCO.
- Warm-up drift is minimal (a few minutes vs 15+ for
  discrete).
- Sawtooth has a visible reset spike -- this is normal for
  a sawtooth-core design.
- Triangle may show slight nonlinearity at the peaks
  (internal conversion from sawtooth).

## Key Takeaways

- The CEM3340 eliminates the hardest parts of VCO design:
  expo converter matching, tempco compensation, and
  precision current sources.
- Your output stage and CV summing network design choices
  are where creativity lives.
- Every classic polysynth used this chip differently --
  study the Electric Druid comparison to understand why.
- Having built a discrete VCO first (labs 07-10), you now
  understand exactly what this chip does internally and
  why it exists.
- This completes the learning path: from voltage dividers
  to a chip-based Eurorack VCO.

## Resources

- [Electric Druid -- CEM3340 Designs](https://electricdruid.net/cem3340-vco-voltage-controlled-oscillator-designs/)
  -- definitive chip-level reference with classic synth
  comparisons
- [AS3340 Datasheet](https://www.alfarzpp.lv/eng/sc/AS3340.php)
- [Kassutronics -- CEM3340 VCO](https://kassu2000.blogspot.com/2018/06/vco-3340.html)
- [Erica Synths -- EDU VCO](https://www.ericasynths.lv/shop/diy-kits-1/edu-diy-vco/)
- [jacobbarssbailey/vco3340](https://github.com/jacobbarssbailey/vco3340)
  -- clean AS3340 Eurorack design with calibration guide
