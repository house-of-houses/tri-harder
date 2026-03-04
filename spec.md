# VCO learning project: a complete resource map

**Every major resource you need for a Eurorack VCO learning project exists and is freely accessible online.** Falstad has built-in examples covering most VCO building blocks (integrator, Schmitt trigger, relaxation oscillator, triangle core), and a community-built full triangle-core VCO simulation targets Eurorack directly. SPICE models for all key components (TL072, LM13700, CA3080, THAT340) are available from manufacturer or vetted community sources. All seven learning URLs have been confirmed live. The one gap: there is **no automated Falstad-to-LTspice export** — manual circuit rebuilding is the standard workflow.

---

## Falstad circuit simulator: built-in examples and community URLs

Falstad's built-in example library covers most VCO building blocks directly. All URLs below use the base `https://www.falstad.com/circuit/`:

**Integrator** — `circuitjs.html?startCircuit=amp-integ.txt` loads a standard op-amp integrator with capacitor feedback. This is the core of any triangle or sawtooth VCO — the ramp-generating stage that converts a constant current into a linear voltage ramp.

**Schmitt trigger** — `circuitjs.html?startCircuit=amp-schmitt.txt` loads an op-amp Schmitt trigger with resistive positive feedback creating hysteresis. This is the comparator stage that detects ramp peaks and resets/reverses the integrator. Falstad also has a built-in CD40106-style Schmitt inverter component under Draw → Active Building Blocks with configurable thresholds.

**Relaxation oscillator** — `e-relaxosc.html` loads the classic RC + op-amp relaxation oscillator. Falstad's own description explains the full charge/discharge cycle: two 100k resistors form a voltage divider putting the + input at half the output voltage, and the capacitor charges through the feedback path until the comparator flips. A GitHub Gist variant using a Schmitt inverter component (element 183) is available at `https://gist.github.com/jrsa/5a3542e4cff018fcc14b0a428b445892` — importable via File → Import.

**Triangle core oscillator** — `e-triangle.html` is the most directly VCO-relevant built-in example. It implements the classic **integrator + Schmitt trigger** topology: the right op-amp integrates (ramping up/down at a rate set by the 10K resistor and capacitor) while the left op-amp acts as a Schmitt trigger switching at dual thresholds. This is the same fundamental topology used in the Moritz Klein VCO and many Eurorack designs.

**Sawtooth / VCO** — `e-vco.html` loads a voltage-controlled oscillator built from an integrator + Schmitt trigger + MOSFET switching. Falstad describes it: "The first op-amp is an integrator... When the MOSFET at the bottom is on, the current goes through the MOSFET... The second op-amp is a Schmitt trigger." A separate **555-based sawtooth** is at `e-555saw.html`. For a more realistic sawtooth core with JFET discharge, the All About Circuits forum thread at `https://forum.allaboutcircuits.com/threads/sawtooth-core-vco.177461/` includes a downloadable Falstad circuit file.

**Exponential converter** — No standalone Falstad simulation exists. The transistor Vbe-Ic exponential relationship that expo converters exploit requires precision modeling beyond Falstad's strengths. However, the community VCO below includes one as part of a complete circuit.

**Full triangle-core VCO for Eurorack** — The most valuable single Falstad resource is a complete triangle-core VCO posted on the Look Mum No Computer forum by user "porl": `https://tinyurl.com/yeskzep5` (redirects to a full `?ctz=` encoded URL). This simulation includes **triangle, saw, pulse, sine, and mix outputs**, exponential and linear FM inputs, sync, an exponential converter with matched transistor pair, and V/Oct and HF compensation trims. Source thread: `https://lookmumnocomputer.discourse.group/t/triangle-core-vco-schematic/3700`.

Additional useful built-in examples include the **differentiator** (`circuitjs.html?startCircuit=amp-dfdx.txt`), **astable multivibrator** (`circuitjs.html?startCircuit=multivib-a.txt`), **sine from triangle via diode shaping** (`e-sinediode.html`), and **Howland current source** (`circuitjs.html?startCircuit=howland.txt`). A complete list of all built-in example filenames is documented in the CircuitJS1 source: `https://github.com/sharpie7/circuitjs1`.

---

## GitHub repositories worth referencing for project structure

Three repositories stand out for an experienced engineer seeking clean project organization:

**musicdevghost/eurorack** (159★, MIT license) at `https://github.com/musicdevghost/eurorack` has the best overall project structure. It includes the **Tom Yum VCO** (fully analog, stable V/oct tracking) alongside other modules. Each module folder contains Gerber files, EasyEDA project files, BOM with direct purchase links (including Tayda auto-cart files), PDF schematics, and video build instructions. This is the template to emulate for folder organization.

**erica-synths/diy-eurorack** at `https://github.com/erica-synths/diy-eurorack` is the industry-standard open-source reference. Contains the **VCO3** (Polivoks-inspired) with per-module folders holding schematics, Gerbers, BOMs, component placement diagrams, and assembly manuals. The companion **mki x es.EDU VCO** — a collaboration with Moritz Klein — comes with a **59-page manual** that walks through every design decision from oscillator core through expo converter, tempco, waveshaping, FM, and PWM. The EDU manual is downloadable from `https://www.ericasynths.lv/shop/diy-kits-1/edu-diy-vco/` and is arguably the single best educational VCO document available.

**PierreIsCoding/sdiy** (98★) at `https://github.com/PierreIsCoding/sdiy` offers the broadest collection — **30+ synth DIY projects** including a CEM3340 VCO, plus an "SDIY Building Blocks" educational overview section. With 602 commits and organized per-project folders, it demonstrates how to structure a multi-module learning repository.

For CEM3340/AS3340-specific designs, the strongest repos are:

- **jacobbarssbailey/vco3340** (`https://github.com/jacobbarssbailey/vco3340`) — clean 6HP Eurorack CEM3340 VCO with detailed calibration procedure
- **polykit/vco-1** (`https://github.com/polykit/vco-1`) — Jupiter 8-inspired discrete transistor VCO using BC847BS matched pair, with excellent references to Kassu's design articles
- **baritonomarchetto/3340-Voltage-Controlled-Oscillator-Module** (`https://github.com/baritonomarchetto/3340-Voltage-Controlled-Oscillator-Module`) — notably includes a **Falstad simulation** of output stages and explains design tradeoffs in detail
- **DIYSynthMNL** (`https://github.com/DIYSynthMNL`) — organization emphasizing thorough design documentation with the stated goal of demystifying how modules work

The curated list at **newdigate/eurorack-awesome** (`https://github.com/newdigate/eurorack-awesome`) is the best single entry point for discovering additional open-source Eurorack projects. A notable gap exists: **no GitHub repo combines a full Eurorack VCO design with comprehensive LTspice simulation files**. Simulation lives primarily in forums and blog posts.

---

## SPICE models: where to get them and how to use them

### TL072 op-amp

The **LTwiki community model** is the easiest path: download `TL072.sub` from `https://ltwiki.org/index.php?title=File:TL072.sub` and place it in LTspice's `lib/sub/` directory. A matching LTspice symbol is at `https://github.com/chrisnoisel/ltspice/blob/master/sym/Opamps/TL072.asy`. The official TI model is available through PSpice for TI (free) at `https://www.ti.com/product/TL072` but requires minor syntax adjustments for LTspice. For precision work, Nazar's high-accuracy macromodels at `https://s-audio.systems` improve DC parameters, PSRR, and 1/f noise modeling beyond the manufacturer model. Be aware that **no TL072 model accurately captures the input phase inversion problem** when inputs approach within ~4V of V-.

### LM13700 OTA

The best LTspice-ready package is at `https://github.com/deanm1278/LM13700-spice-model`, which includes `lm13700.sub`, `LM13700.asy` (symbol), and `lm13700_test.asc` (test circuit). This is derived from the original transistor-level model by **Don Sauer**, co-inventor of the LM13700, hosted at `http://www.idea2ic.com/LM13600/SpiceSubcircuit/LM13700_SpiceModel.html`, modified with a VSS terminal for split-supply operation. A complete LM13700 OTA VCO simulation at ±5V is available at YouSpice: `https://youspice.com/spiceprojects/spice-simulation-projects/signal-generation-circuits-spice-simulation-projects/lm13700-ota-voltage-controlled-oscillator-5-volt-power/`. Note that real LM13700s have significant part-to-part variation in DC offset that models won't reveal.

### CA3080 OTA

The best available model is a transistor-level subcircuit from the diyAudio "Lost in SPICE" thread at `https://www.diyaudio.com/community/threads/lost-in-spice-rare-obsolete-models.299628/`, which includes custom NPN/PNP transistor models matched to the CA3080 die. Copy the `.SUBCKT` block into a `CA3080.sub` file for LTspice. Additional models exist in the LTspice users group file library at `https://groups.io/g/LTspice/topic/ota_ca3080_models_in/50201173` (free account required). The CA3080 is discontinued (originally RCA); the LM13700 is its modern successor with a different internal architecture.

### THAT340 matched transistor pair

**Official manufacturer SPICE models** are available directly from THAT Corporation at `https://thatcorp.com/device-models/` — a single ZIP download contains models, symbols, and test circuits for the entire 300 series. These simulate typical noise, offset voltage (**500µV matching**), bias current, and gain bandwidth. The datasheet is at `https://thatcorp.com/datashts/THAT_300-Series_Datasheet.pdf`. For expo converter simulation, you can alternatively use **identical instances of any standard BJT model** (e.g., two 2N3904s with the same `.MODEL` parameters) — the critical requirement is parameter identity, not THAT340-specific behavior.

### Tempco resistors

**No special model file is needed.** LTspice's built-in resistor temperature coefficient handles this natively. Set the `tc1` parameter on any resistor:

```
R1 node1 node2 1k tc1=3.353e-3 tc2=0
```

The value **3353 ppm/K** equals 1/T_absolute (1/298.15K at 25°C) — the ideal tempco for canceling the BJT Vbe temperature coefficient in an expo converter. Run `.step temp 0 50 5` to sweep temperature and verify compensation. For non-linear behavior, use a behavioral source: `B1 n1 n2 I=V(n1,n2)/(1000*(1+3.353e-3*(Temp-25)))`. Real tempcos are typically **3300 or 3500 ppm/K** with ±10% tolerance — simulate both extremes. Ian Fritz's tempco equations at `http://ijfritz.byethost4.com/sy_cir7.htm` provide the mathematical foundation.

### Complete VCO simulation package

The best turnkey LTspice VCO package is Dave Erickson's DIY Eurorack synth files at `https://www.djerickson.com/synth/files/DIY_Synth_Files.zip` — includes complete simulations for VCO, VCA, and VCFs with all required SPICE models bundled. His candid caveat applies broadly: "Precision circuits that require low offset voltages, good matching, tight gain control... all simulate wonderfully, but the reality is that transistors don't match, amplifiers have offsets, and gains vary from part-to-part."

---

## All seven learning resource URLs confirmed live

| Resource | Confirmed URL | Status |
|----------|---------------|--------|
| Moritz Klein VCO series | `https://www.youtube.com/@MoritzKlein0` | ✅ Live — 4-part series + extras |
| Lanterman / Lantertronics | `https://www.youtube.com/@Lantertronics` | ✅ Live — ECE4450 lectures |
| René Schmitz expo tutorial | `https://www.schmitzbits.de/expo_tutorial/` | ✅ Live |
| North Coast Synthesis | `https://northcoastsynthesis.com/` | ✅ Live (phasing out modules) |
| Kassutronics VCO | `https://kassu2000.blogspot.com/2018/06/vco-3340.html` | ✅ Live |
| Thomas Henry VCO-1 | `https://www.birthofasynth.com/Thomas_Henry/Pages/VCO-1.html` | ✅ Live |
| Electric Druid CEM3340 | `https://electricdruid.net/cem3340-vco-voltage-controlled-oscillator-designs/` | ✅ Live |

**Moritz Klein** has a 4-part VCO series covering oscillator core, exponential converter, tuning/temperature stability, and waveshaping, plus additional videos on PWM and a 3-oscillator module build. The companion mki x es.EDU kit manual goes deeper than the YouTube series. Videos are individual uploads on his channel (no single playlist URL confirmed).

**Aaron Lanterman's** Georgia Tech courses are the most academically rigorous resource. The faculty page at `https://lanterman.ece.gatech.edu/` links to course pages for ECE4803B (2006) and ECE4893A (2008, 2010) covering sawtooth-core VCOs, triangle-core VCOs, waveshaping, sync, and FM — with university-level mathematical treatment. The 2020–2021 ECE4450 lectures are on the YouTube channel.

**René Schmitz's expo tutorial** at `https://www.schmitzbits.de/expo_tutorial/` remains one of the most-referenced resources in synth DIY. It covers Ebers-Moll equations, transistor pair compensation, tempco resistors (Pt 1kΩ), heated arrays (µA726, CA3046), and alternative compensation methods. His VCO 4069 design (`schmitzbits.de/vco4069.html`) is widely cloned.

**North Coast Synthesis** stands out for mathematical rigor. Their MSK 013 Middle Path VCO uses an unconventional AD633 analog multiplier + THAT320 approach to exponential conversion. The temperature compensation article at `northcoastsynthesis.com/news/temperature-compensation-with-ntc-thermistors/` is outstanding. Full schematics are under GNU GPL. Note: North Coast announced it is **phasing out Eurorack modules** as a business, though documentation remains available.

**Kassutronics** has both a CEM3340-based VCO (`kassu2000.blogspot.com/2018/06/vco-3340.html`) — one of the best-documented AS3340 designs, used in 698+ ModularGrid racks — and a discrete sawtooth-core VCO in two parts covering the core with expo converter (Part 1, December 2015) and waveshaping (Part 2, February 2016).

**Thomas Henry's VCO-1** at birthofasynth.com is a CA3080 OTA-based triangle-core VCO with 9-octave range. The resources page discusses sourcing 2K tempco resistors and THAT Corp matched pairs. PDF schematics are directly downloadable.

**Electric Druid's CEM3340 article** is the definitive chip-design reference. It analyzes annotated schematics from the **Prophet 5 Rev.3, OB-Xa, Jupiter-6, MKS-80, and Memorymoog** — showing how each manufacturer handled the same chip differently. It covers output level quirks (ramp 0–10V, tri 0–5V, pulse 0–13.7V), sync circuits, and differences between CEM3340 Rev.G, CoolAudio V3340, and Alfa AS3340.

---

## Falstad-to-LTspice workflow: manual rebuild is the only path

**There is no native export or automated conversion between Falstad and LTspice.** CircuitJS1 uses a proprietary text-based format — its `File > Export as Text` produces Falstad-specific encoding, not SPICE netlists. No community converter tool exists.

The practical workflow used across the synth DIY community is a two-stage approach:

1. **Falstad for topology exploration** — use it to quickly test whether an integrator-Schmitt trigger combination oscillates, visualize current flow through feedback paths, experiment with component values, and build intuition about circuit behavior. The animated current visualization is unmatched for understanding.

2. **LTspice for accurate simulation** — manually recreate the circuit when you need real component models, temperature sweeps, noise analysis, or precise frequency/amplitude predictions. Add proper op-amp models (TL072), transistor models (THAT340 or matched pairs), and use `.step temp` for tempco verification.

The University of Toronto PHY405 lab at `https://www.physics.utoronto.ca/apl/405/Lab_S.html` explicitly teaches this workflow — students build in Falstad first, then reproduce in LTspice. Iain Sharp, the major CircuitJS1 contributor, confirms the tools serve fundamentally different purposes: Falstad for visualization and intuition, SPICE for quantitative accuracy.

**LTspice convergence tips specific to VCO circuits**: Use `.tran 0 50m 0 1u uic` (the `uic` flag skips DC operating point calculation, which often fails for oscillator circuits). Ramp supply voltages with PWL sources rather than instantaneous steps. Add small initial conditions (`.ic V(node)=0.1`) to kick-start oscillation. For LM13700 positive-feedback paths, increase tolerances with `.options reltol=0.01` or switch to `.options method=gear` if "timestep too small" errors appear.

---

## Conclusion

The VCO learning ecosystem is rich but fragmented — no single repository combines simulation files, progressive tutorials, and production-ready Eurorack designs. The strongest educational path chains **Moritz Klein's videos** (intuition) → **Falstad built-in examples** (interactive visualization of each building block) → **the LMNC community's full triangle-core VCO Falstad simulation** (complete system) → **LTspice with proper models** (quantitative verification) → **Electric Druid and Lanterman** (deep chip-level and academic understanding). For project structure, **musicdevghost/eurorack** and **erica-synths/diy-eurorack** set the standard. The identified gap — no GitHub repo with integrated LTspice simulations alongside a complete Eurorack VCO build — represents an opportunity: a learning project that fills this gap with progressive Falstad examples, matched LTspice simulations, and a final KiCad PCB would be genuinely novel in this space.