# SPICE Models

## When you need external models

Labs 01-06 use Falstad only -- no SPICE models needed.

Labs 07-10 can run entirely with **LTspice built-in
components** (`opamp2`, `2N3904`, `2N3906`). External models
are only needed for higher accuracy.

Lab 11 uses behavioral sources to simulate CEM3340 outputs
(no chip-level SPICE model exists publicly).

## TL072 Op-Amp

The LTspice built-in `opamp2` works for all labs. For more
realistic simulation:

1. Download `TL072.sub` from
   [LTwiki](https://ltwiki.org/index.php?title=File:TL072.sub)
2. Place in your LTspice `lib/sub/` directory
3. Download matching symbol from
   [chrisnoisel/ltspice](https://github.com/chrisnoisel/ltspice/blob/master/sym/Opamps/TL072.asy)
4. Place in `lib/sym/Opamps/`

Typical LTspice library paths:

- **macOS**: `~/Library/Application Support/LTspice/lib/`
- **Windows**: `C:\Users\<name>\Documents\LTspiceXVII\lib\`

Note: no TL072 model accurately captures the input phase
inversion when inputs approach within ~4V of V-.

## LM13700 OTA

Not used in the default lab circuits but useful for
OTA-based VCO variations.

1. Clone or download from
   [deanm1278/LM13700-spice-model](https://github.com/deanm1278/LM13700-spice-model)
2. Contains: `lm13700.sub`, `LM13700.asy`, and
   `lm13700_test.asc`
3. Place `.sub` in `lib/sub/` and `.asy` in `lib/sym/`

Derived from Don Sauer's transistor-level model (co-inventor
of the LM13700). Modified for split-supply operation.

## THAT340 Matched Transistor Pair

For precision expo converter simulation (lab 08 advanced):

1. Download from
   [THAT Corporation](https://thatcorp.com/device-models/)
2. Single ZIP contains models for the entire 300 series
3. Extract and place in `lib/sub/`

For basic expo converter work, **matched 2N3904 instances**
(same `.MODEL` parameters) simulate the critical behavior
adequately. The THAT340's advantage is real-world matching
precision (500uV offset), not simulation accuracy.

## Tempco Resistors

No special model file needed. Use LTspice's built-in
temperature coefficient parameter on any resistor:

```spice
R1 node1 node2 1k tc1=3.353e-3 tc2=0
```

The value 3353 ppm/K equals 1/T_absolute (1/298.15K at
25C), matching the BJT Vbe temperature coefficient.

To verify compensation, run a temperature sweep:

```spice
.step temp 0 50 5
```

Real tempco resistors are typically 3300 or 3500 ppm/K
with +/-10% tolerance. Simulate both extremes to
understand the sensitivity.

## Directory structure

```text
spice-models/
├── README.md          <-- this file
├── TL072/             <-- place TL072.sub and .asy here
├── LM13700/           <-- place LM13700 files here
└── THAT340/           <-- place THAT 300-series files here
```

After downloading, copy the relevant files into LTspice's
library directories as described above. The folders here
serve as local backup and project reference.
