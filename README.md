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

- `C5_models.txt` — BSIM3 model file (copy-paste into your SPI files)
- Each experiment folder contains the `.spi` netlist file ready for LTSpice

## Step-by-Step Procedure (Run SPI Files in LTSpice)

### Prerequisites
1. Install **Java JDK 17+** — download from [Oracle](https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html)
2. Install **LTSpice** — download from [Analog Devices](https://www.analog.com/en/design-center/design-tools-and-calculators/ltspice-simulator.html)

### Step 1: Download Files
1. Click **Code → Download ZIP** on this repo, or download individual `.spi` files
2. Download `C5_models.txt` from this repo
3. Keep all files in one folder (e.g., `CMOS_Lab/`)

### Step 2: Open SPI File in LTSpice
1. Open **LTSpice**
2. Go to **File → Open**
3. Navigate to the experiment folder
4. Change file type dropdown from `Symbo (*.asc)` to **Netlist (*.spi;*.net;*.cir)**
5. Select the `.spi` file (e.g., `inv_schematic.spi`)
6. Click **Open**

### Step 3: Add C5 Models (if not already included)
If the SPI file doesn't include C5 models, add them manually:
1. Open the `.spi` file in **Notepad**
2. Delete everything between `.OPTIONS` and `.MODEL DIFFCAP` (all Electric-generated models)
3. Delete `.GLOBAL gnd vdd`
4. Open `C5_models.txt`, copy the entire content
5. Paste it just before `.END` in the SPI file
6. Save the file

### Step 4: Add Voltage Sources (if not already included)
If the SPI file doesn't have voltage sources, add them before `.END`:
```
Vdd vdd gnd 5
VA A gnd PULSE(0 5 0 10p 10p 10n 20n)
.tran 60ns
```
- `Vdd` — 5V DC supply
- `VA` — Square wave input (0→5V, 20ns period)
- `.tran 60ns` — Simulate for 60 nanoseconds

### Step 5: Fix Common Errors
| Error | Fix |
|---|---|
| `More than one analysis specified` | Remove duplicate `.tran` lines — keep only one |
| `Undefined node` | Check node names match between circuit and voltage sources |
| `TRANS` error | Change to `TRAN` (correct spelling) |
| Model mismatch | Make sure model names in circuit (NMOS/PMOS) match `.MODEL` names in C5 file |

### Step 6: Simulate
1. Click **Run** (running man icon) in LTSpice
2. A blank waveform window opens
3. **Click on a wire** in the schematic to add a trace (e.g., input `A` or output `Y`)
4. View waveforms — verify the circuit works

### Step 7: Measure Results (Exp 7-10)
- **Propagation Delay:** Use cursor to measure time between 50% input and 50% output
- **Power:** Plot `V(vdd)*I(Vdd)` in the waveform window
- **Id-Vg / Id-Vd:** Use `.DC` sweep commands in the SPI file

## Quick Reference: SPI File Structure
```
* Title
M1 Y A gnd gnd NMOS L=0.6U W=3U
M2 Y A vdd vdd PMOS L=0.6U W=6U
Vdd vdd gnd 5
VA A gnd PULSE(0 5 0 10p 10p 10n 20n)

.tran 60ns

* C5 BSIM3 Models
.MODEL NMOS NMOS (LEVEL=8 ...)
.MODEL PMOS PMOS (LEVEL=8 ...)

.END
```

## Tools Used
- [Electric VLSI](http://www.electricvlsi.org/) — Schematic & Layout editor
- [LTSpice](https://www.analog.com/en/design-center/design-tools-and-calculators/ltspice-simulator.html) — Circuit simulation
- [CMOS EDU](https://cmosedu.com/) — C5 process models

## Video Tutorial
Full video tutorial coming soon on [YouTube](https://youtube.com/@forgeVII).
