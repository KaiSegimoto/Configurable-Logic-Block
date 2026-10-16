# Configurable Logic Block

A transistor-level **FPGA configurable logic block (CLB)** containing one basic logic element. It has a 16-bit 6T SRAM configuration array, a serial-load shift register, a 4-input pass-transistor look-up table (LUT) and an output D flip-flop. It was designed in **45 nm CMOS** with Cadence Virtuoso.

Final project for **ESE 5700 – Digital Integrated Circuits and VLSI**, University of Pennsylvania, Fall 2024.
Author: **Kai Segimoto**

## Interface

- **Inputs:** `clk`, `load` (SRAM write enable), `data` (serial configuration bitstream), `LUT select<3:0>`
- **Output:** `Out`

## Design

- **6T SRAM array**: cross-coupled inverters with access transistors, sized for read stability and writability. Bitlines are driven by tristate buffer/inverter pairs. Precharge is gated by a NOR of write-enable and word line.
- **Two-phase non-overlapping clock generator**: drives the clock lines, bitline drivers and SRAM.
- **Shift register**: loads the serial bitstream into the SRAM array.
- **4-input LUT**: a tree of 15 2:1 pass-transistor muxes reading the SRAM contents.
  - Removing unnecessary inverter stages saved 26 transistors.
  - That fix sped up the critical path and cut both energy and area.
- **D flip-flop**: registers the LUT output.

## Results

| Metric | Result |
|---|---|
| Max operating frequency | **1.176 GHz** (850 ps clock) |
| Loading energy | 14.35 pJ |
| Active energy | 7.818 pJ |
| Average energy (20 % load / 80 % active) | **9.12 pJ** |
| Area (sum of transistor widths) | 46.88 µm |
| Figure of merit (area × energy × delay) | 3.63 × 10⁻²⁵ m·J·s |

## Files

- `Proj 2 Report.docx`: full design report with schematics, transistor sizing, functional and timing simulations, and the energy, area and FoM calculations

Download the report to view it, since GitHub does not render `.docx` files.
