# CMOS VLSI Design Lab — Electric VLSI + LTSpice

SPI netlist files for all 10 experiments using **AMI C5 (0.5µm) BSIM3 models** in **Electric VLSI** + **LTSpice**.

## Process: AMI C5
- **VDD:** 5V
- **Min L (drawn):** 2 → 0.6µm actual
- **Min W (drawn):** 10 → 3µm actual
- **Model:** BSIM3 Level 8

## Experiments

| # | Experiment | Status |
|---|---|---|
| 1 | CMOS Inverter Schematic | Done |
| 2 | CMOS Inverter Layout | Done |
| 3 | 2-input NAND Schematic | Done |
| 4 | 2-input NAND Layout | Done |
| 5 | 2-input NOR Schematic | Done |
| 6 | 2-input NOR Layout | Done |
| 7 | Propagation Delay Measurement | Coming Soon |
| 8 | Power Dissipation Measurement | Coming Soon |
| 9 | Id-Vg Characteristics | Coming Soon |
| 10 | Id-Vd Characteristics | Coming Soon |

## Files

- `C5_models.txt` — BSIM3 model file (download and paste into your SPI files)
- Each experiment folder contains the `.spi` netlist file

## How to Use

1. Download the `.spi` file for your experiment
2. Download `C5_models.txt`
3. Open the `.spi` file in LTSpice (File → Open → select file type Netlist)
4. Simulate and view waveforms

## Tools Used
- [Electric VLSI](http://www.electricvlsi.org/) — Schematic & Layout editor
- [LTSpice](https://www.analog.com/en/design-center/design-tools-and-calculators/ltspice-simulator.html) — Circuit simulation
