# Resources

A curated reading and viewing list for working through the labs from a
no-electronics-background starting point. Organized by phase rather than
by topic: read in roughly this order alongside the labs.

## Already linked from the README

For completeness; see the README for the short list and how each maps to
the labs.

- [Moritz Klein -- VCO series (YouTube)](https://www.youtube.com/playlist?list=PLHeL0JWdJLvT1PAqW4TtvxtRoXyk741WM)
- [Aaron Lanterman -- Lantertronics ECE lectures](https://lanterman.ece.gatech.edu/)
- [Rene Schmitz -- Expo converter tutorial](https://www.schmitzbits.de/expo_tutorial/)
- [Electric Druid -- CEM3340 article](https://electricdruid.net/cem3340-vco-voltage-controlled-oscillator-designs/)
- [Erica Synths -- mki x es.EDU VCO manual](https://web.archive.org/web/20241014024000/https://www.ericasynths.lv/media/VCO_MANUAL_v2.pdf)
- [North Coast Synthesis -- VCO design notes](https://northcoastsynthesis.com/news/)
- [Kassutronics -- VCO build](https://kassu2000.blogspot.com/2018/06/vco.html)

## Phase 1: Before lab 1

- **Make: Electronics** (3rd ed.) -- Charles Platt. Book, ~$30. Cover-to-cover
  hands-on intro that gets you to op-amps; matches labs 01-05 in spirit.
- **Engineer's Mini-Notebook: Op-Amp IC Circuits** -- Forrest Mims III.
  [Free PDF](https://github.com/alaricmoore/MiniEngineeringNotebooks). 32-page
  hand-drawn op-amp reference, useful as a bench cheat sheet.
- **Getting Started in Electronics** -- Forrest Mims III. Book, ~$20. Compact
  alternative to Platt; reference-style.

## Phase 2: Op-amp and feedback labs (03-06)

- **Learning the Art of Electronics** (2nd ed.) -- Hayes & Horowitz. Book,
  ~$80. Lab-course companion to AoE; covers the same ground as labs 01-06
  with more rigor than Klein.
- **Op Amps for Everyone** (5th ed.) -- Mancini & Carter.
  [Free PDF](https://web.mit.edu/6.101/www/reference/op_amps_everyone.pdf).
  The accessible op-amp reference.
- **Make: More Electronics** -- Charles Platt. Book, ~$30. Positive-feedback
  and oscillator chapters back labs 05-06.

## Phase 3: VCO core, expo, waveshaping (07-11)

- **The Art of Electronics** (3rd ed.) -- Horowitz & Hill. Book, ~$100.
  Reference for chapters 2 (BJT, Ebers-Moll), 4 (op-amps), 7 (precision); the
  deep dive when Klein hand-waves.
- **Designing Analog Chips** -- Hans Camenzind.
  [Free PDF](http://www.designinganalogchips.com/). Current mirrors, diff
  pairs, bandgaps: the inside of the CEM3340 made readable.
- **Op Amp Applications Handbook** -- Walt Jung (ed.), Analog Devices.
  [Free PDF](https://www.analog.com/en/resources/technical-books/op-amp-applications-handbook.html).
  Doorstop reference for op-amp selection.
- **Practical Electronics for Inventors** (4th ed.) -- Scherz & Monk. Book,
  ~$40. Bridges Platt to AoE; useful BJT/expo coverage for lab 08.

## Synth canon: deeper references

- **Tim Stinchcombe's synth pages** --
  [timstinchcombe.co.uk](https://www.timstinchcombe.co.uk). Deepest public
  analysis of CEM3340 internals and triangle-to-sine shaping.
- **Yusynth** -- Yves Usson. [yusynth.net](https://yusynth.net). Full
  schematics with theory of operation for every classic module type.
- **CGS / Ken Stone** -- archived at
  [sdiy.info/wiki/CatGirl_Synth](https://sdiy.info/wiki/CatGirl_Synth).
  Serge-derived module designs; complements East-Coast bias of Klein/MFOS.
- **Ian Fritz** -- [ijfritz.byethost4.com](https://ijfritz.byethost4.com).
  Chaos oscillators, thru-zero VCOs, hyperbolic shapers; where to go after
  lab 11.
- **Thomas Henry: Making Music with the 3080 OTA** -- via Magic Smoke
  Electronics. ~$15. Companion path into VCAs and OTA-based filters.
- **MFOS (Music From Outer Space)** -- Ray Wilson.
  [musicfromouterspace.com](https://musicfromouterspace.com). Full archive
  beyond the book: schematics, calibration walkthroughs.
- **Doepfer A-100 DIY pages** --
  [doepfer.de/DIY/a100_diy.htm](https://doepfer.de/DIY/a100_diy.htm). 50+ page
  tutorial through reading schematics and building a VCO/VCF/VCA chain.
- **J. Donald Tillman** -- [till.com/articles](http://www.till.com/articles/).
  Brief but rigorous notes on filters and exponential decay.
- **Schmitz full site** -- [schmitzbits.de](https://www.schmitzbits.de). Beyond
  the expo tutorial: ADSR, VCA, S&H, envelope follower.
- **SDIY Wiki** -- [sdiy.info](https://sdiy.info). Community-maintained
  encyclopedia; use as the entry-point hub.

## Musical and theoretical context

- **Synth Secrets** -- Gordon Reid, Sound on Sound. Free, 63 articles:
  [soundonsound.com](https://www.soundonsound.com/series/synth-secrets-sound-sound).
  The musical "why" behind every circuit you build.
- **Handmade Electronic Music** (3rd ed.) -- Nicolas Collins. Book, ~$45.
  Hex-Schmitt and circuit-bending projects musically frame lab 06.
- **Electronic Music: Systems, Techniques, and Controls** -- Allen Strange.
  Book, ~$50. Canonical patching and systems pedagogy.
- **Musical Applications of Microprocessors** -- Hal Chamberlin. Out of print,
  borrowable:
  [archive.org](https://archive.org/details/musicalapplicati0002cham). Section
  2 is the most rigorous analog-module textbook treatment ever printed.

## Magazine archives

- **mu:zines** -- [muzines.co.uk](https://www.muzines.co.uk). Complete legal
  Polyphony 1975-1985 archive plus E&MM, SOS early issues.
- **World Radio History** --
  [worldradiohistory.com](https://www.worldradiohistory.com). Polyphony,
  Electronic Musician, ETI, Practical Electronics, Radio-Electronics archives.
- **Elektor Formant series** --
  [emusic-diy.org/ForMant](https://www.emusic-diy.org/ForMant). Original
  1977-78 articles for the Formant modular.
- **Bernie Hutchins / Electronotes** --
  [electronotes.netfirms.com](http://electronotes.netfirms.com). Free
  materials only (back issues no longer sold; full PDFs in circulation are
  copyright-infringing).

## SPICE and bench skills

- **The LTspice IV Simulator** -- Gilles Brocard.
  [Free PDF (DigiKey)](https://media.digikey.com/pdf/Data%20Sheets/Wurth%20Electronics%20PDFs/LTspice_IV_Simulator_Appl%20Handbook.pdf).
  The reference for using LTspice as a thinking tool.
- **Troubleshooting Analog Circuits** -- Bob Pease. Book or PDF circulating
  online. When the breadboard works and the PCB does not.
- **PyLTSpice** --
  [github.com/nunobrum/PyLTSpice](https://github.com/nunobrum/PyLTSpice).
  Python automation for parameter sweeps; relevant to existing SPICE workflow.
- **EEVblog Fundamentals Friday** -- Dave Jones, YouTube. Bench-skill basics:
  scope, multimeter, op-amp probing.

## Communities

- **SDIY mailing list** -- [synth-diy.org](https://synth-diy.org). Active;
  archives searchable. Where Hutchins, Stinchcombe, Skala, Rossum discuss
  specifics.
- **ModWiggler DIY subforum** --
  [modwiggler.com](https://www.modwiggler.com). The Stack Overflow of synth
  DIY since ~2007.
- **Look Mum No Computer forum** --
  [lookmumnocomputer.discourse.group](https://lookmumnocomputer.discourse.group).
  Friendlier and more beginner-tolerant than ModWiggler.

## Status and link health

A few links above are on intermittent or fragile hosting. If a link is dead,
try the [Wayback Machine](https://web.archive.org) first.

- Electronotes is on intermittent hosting; use Wayback snapshots if down.
- Ian Fritz is on a free host (byethost4); also use Wayback if down.
- PAiA's main site is intermittent as of 2025; Polyphony content lives at
  mu:zines and World Radio History.
- CGS / Ken Stone main site redirects to community archives; Stone retired in
  2016.
- The Op Amp Applications Handbook page on analog.com is bot-protected; visit
  in a browser, not via curl or scripts.
- The Erica Synths VCO manual entry above points to a Wayback snapshot
  because the original `ericasynths.lv/media/VCO_MANUAL_v2.pdf` returns 404
  as of 2026-05.
