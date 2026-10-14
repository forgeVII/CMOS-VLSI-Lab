# CMOS VLSI Design Lab — Electric VLSI + LTSpice

SPI netlist files for the CMOS VLSI Design Lab using **AMI C5 (0.5µm) BSIM3 models** in **Electric VLSI** + **LTSpice**.

## Process: AMI C5
- **VDD:** 5V
- **Min L (drawn):** 2 → 0.6µm actual
- **Min W (drawn):** 10 → 3µm actual
- **Model:** BSIM3 Level 8

## Model Files (two options)

| File | NMOS | PMOS | Notes |
|---|---|---|---|
| `C5_models.txt` | L=0.6U W=3U | L=0.6U W=6U | Standard C5 sizes (video/series default) |
| `C5_models_L04_W08_W16.txt` | L=0.4U W=0.8U | L=0.4U W=1.6U | Extra option (smaller devices) |

Both are C5 BSIM3 Level-8 model cards — pick the one that matches the transistor sizes you drew in Electric. Copy-paste the contents into your `.spi` file before `.END`.

> *Note on the small-size file:* L=0.4U / W=0.8U are below the C5 model minimum (L≥0.6U, W≥3U). They still simulate in LTSpice, but the standard `C5_models.txt` sizes are recommended for accurate results.

## Experiments

| # | Experiment | Folder | Status |
|---|---|---|---|
| 1 | CMOS Inverter Schematic | `Exp01_Inverter_Schematic` | Done |
| 2 | CMOS Inverter Layout | `Exp02_Inverter_Layout` | Done |
| 3 | 2-input NAND Schematic | `Exp03_NAND_Schematic` | Done |
| 4 | 2-input NAND Layout | `Exp04_NAND_Layout` | Done |
| 5 | 2-input NOR Schematic | `Exp05_NOR_Schematic` | Done |
| 6 | 2-input NOR Layout | `Exp06_NOR_Layout` | Done |
| 7 | XOR Gate | `Exp07_XOR` | Done |
| 8 | XNOR Gate | `Exp08_XNOR` | Done |
| 9 | Power Dissipation | `Exp09_Power_Dissipation` | Pending |
| 10 | Propagation Delay | `Exp10_Propagation_Delay` | Done |
| 11 | NMOS Id-Vg Characteristics | `Exp11_Id_Vg` | Done |
| 12 | NMOS Id-Vd Characteristics | `Exp12_Id_Vd` | Done |

## Files

- `C5_models.txt` — BSIM3 model file (standard sizes: Wn=3U, Wp=6U)
- `C5_models_L04_W08_W16.txt` — BSIM3 model file (smaller devices: Wn=0.8U, Wp=1.6U)
- Each experiment folder contains the `.spi` netlist file ready for LTSpice

## Step-by-Step Procedure (Run SPI Files in LTSpice)

### Prerequisites
1. Install **Java JDK 17+** — download from [Oracle](https://www.oracle.com/in/java/technologies/downloads/)
2. Install **LTSpice** — download from [Analog Devices](https://www.analog.com/en/design-center/design-tools-and-calculators/ltspice-simulator.html)

### Step 1: Download Files
1. Click **Code → Download ZIP** on this repo, or download individual `.spi` files
2. Download the model file that matches your transistor sizes
3. Keep all files in one folder (e.g., `CMOS_Lab/`)

### Step 2: Open SPI File in LTSpice
1. Open **LTSpice**
2. Go to **File → Open**
3. Navigate to the experiment folder
4. Change file type dropdown from `Symbol (*.asc)` to **Netlist (*.spi;*.net;*.cir)**
5. Select the `.spi` file (e.g., `inv_schematic.spi`)
6. Click **Open**

### Step 3: Add C5 Models (only if not already included)
Most `.spi` files in this repo are **self-contained** — the C5 models are already inside them, so you can skip this step.
If your netlist is missing models:
1. Open the `.spi` file in **Notepad**
2. Delete everything between `.OPTIONS` and `.MODEL DIFFCAP` (all Electric-generated models)
3. Delete `.GLOBAL gnd vdd`
4. Open the model file, copy the entire content
5. Paste it just before `.END` in the SPI file
6. **Name check:** the model files define `.MODEL NMOS` / `.MODEL PMOS`. If your
   netlist devices use `N` / `P`, rename the model definitions to
   `.MODEL N NMOS` and `.MODEL P PMOS` (or rename the devices to `NMOS`/`PMOS`).
7. Save the file

### Step 4: Add Voltage Sources (if not already included)
If the SPI file doesn't have voltage sources, add them before `.END`.

**Inverter (1 input):**
```
Vdd vdd gnd 5
VA A gnd PULSE(0 5 0 10p 10p 10n 20n)
.tran 60ns
```

**NAND / NOR / XOR / XNOR (2 inputs):**
```
Vdd vdd gnd 5
VA A gnd PULSE(5 0 0 10p 10p 10n 20n)
VB B gnd PULSE(5 0 0 10p 10p 20n 40n)
.tran 80ns
```

- `Vdd` — 5V DC supply
- `VA`, `VB` — square wave inputs (staggered 20ns)
- `.tran` — simulation length

### Step 5: Fix Common Errors
| Error | Fix |
|---|---|
| `More than one analysis specified` | Remove duplicate `.tran` lines — keep only one |
| `Undefined node` | Check node names match between circuit and voltage sources |
| `Duplicate name` | Two voltage sources can't share a name (use VA and VB, not VA twice) |
| `TRANS` error | Change to `TRAN` (correct spelling) |
| Model mismatch | Make sure model names in circuit (N/P or NMOS/PMOS) match `.MODEL` names |

### Step 6: Simulate
1. Click **Run** (running man icon) in LTSpice
2. A blank waveform window opens
3. **Click on a wire** in the schematic to add a trace (e.g., input `A` or output `Y`)
4. View waveforms — verify the circuit works

### Step 7: Measure Results (Exp 9-12)

| Exp | Analysis | Command |
|---|---|---|
| 10 — Propagation Delay | `.tran` + cursors | Measure time between 50% input and 50% output |
| 9 — Power Dissipation | `.tran` | Plot `V(vdd)*I(Vdd)`, average over a period |
| 11 — Id-Vg (transfer) | `.dc` | `.dc Vgs 0 5 0.01` (with `Vds` fixed at 5V) |
| 12 — Id-Vd (output) | `.dc` | `.dc Vds 0 5 0.01 Vgs 0 5 1` (nested sweep) |

## Quick Reference: SPI File Structure
```
* Title
M1 Y A gnd gnd N L=0.6U W=3U
M2 Y A vdd vdd P L=0.6U W=6U
Vdd vdd gnd 5
VA A gnd PULSE(0 5 0 10p 10p 10n 20n)

.tran 60ns

* C5 BSIM3 Models
.MODEL N NMOS (LEVEL=8 ...)
.MODEL P PMOS (LEVEL=8 ...)

.END
```

## Tools Used
- [Electric VLSI](http://www.electricvlsi.org/) — Schematic & Layout editor
- [LTSpice](https://www.analog.com/en/design-center/design-tools-and-calculators/ltspice-simulator.html) — Circuit simulation
- [CMOS EDU](https://cmosedu.com/) — C5 process models

## Video Tutorial
Full video tutorial series on YouTube — **CMOS VLSI Design Lab**:
https://www.youtube.com/playlist?list=PLcglmh7VEeDE
