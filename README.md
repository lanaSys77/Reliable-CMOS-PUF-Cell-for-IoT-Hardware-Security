# Reliable CMOS PUF Cell for IoT Hardware Security

## Overview

This project presents a reliable CMOS SRAM-based Physically Unclonable Function (PUF) for IoT hardware security. The design uses a 6T SRAM cell as the entropy source and integrates a Schmitt Trigger assisted readout circuit to improve output stability and noise immunity.

The complete architecture was implemented hierarchically using Electric VLSI and verified through NGSpice simulations. The system includes a CMOS inverter, 6T SRAM PUF cell, Schmitt Trigger sensing circuit, 1-bit readout circuit, Word Line Decoder, Bit Detector, and a 4×4 SRAM PUF array.

## Repository Description

CMOS SRAM-based PUF for IoT hardware security using Schmitt Trigger assisted readout, designed in Electric VLSI and verified with NGSpice.

## Team Members

- Sadeen Faqih
- Nadeen Jaber
- Lana Sayes
- Julnar Assi

Department of Electrical and Computer Engineering  
Birzeit University  
Digital Integrated Circuits Project

## Project Motivation

IoT devices require lightweight and secure authentication methods. Traditional security systems usually store secret keys in memory, which can expose the device to physical attacks such as probing, reverse engineering, and memory extraction.

Physically Unclonable Functions (PUFs) solve this issue by generating unique hardware fingerprints from natural CMOS manufacturing variations instead of storing permanent secret keys.

In SRAM-based PUFs, each SRAM cell powers up to either logic 0 or logic 1 based on transistor mismatch. This startup value can be used as a unique response bit.

However, weak SRAM cells may produce unstable responses due to noise, temperature variation, supply voltage changes, or metastability. To improve reliability, this project uses a Schmitt Trigger assisted readout stage.

## What is a PUF?

A Physically Unclonable Function (PUF) is a hardware security primitive that generates a unique response based on physical manufacturing variations.

Even if two chips are designed with the same layout, small manufacturing differences make each chip behave slightly differently. These differences can be used to generate a unique hardware fingerprint.

In this project, the SRAM cell startup state is used as the PUF response.

## Why SRAM PUF?

SRAM PUFs are useful because:

- SRAM cells already exist in many digital systems.
- No permanent secret key needs to be stored.
- The startup state depends on natural transistor mismatch.
- The design is suitable for lightweight IoT security.
- The response can be used for device authentication or key generation.

## System Architecture

The proposed system follows this signal flow:

Address → Word Line Decoder → 4×4 SRAM PUF Array → BL/BLB Bitlines → Schmitt Trigger Assisted Readout → Bit Detector → PUF Output

The architecture generates a 16-bit PUF response using a 4×4 SRAM array. Each SRAM cell contributes one response bit.

## Main Circuit Blocks

### 1. CMOS Inverter

The CMOS inverter is the basic building block of the design.

It consists of one PMOS transistor in the pull-up network and one NMOS transistor in the pull-down network.

When the input is low, the PMOS turns ON and the output becomes HIGH. When the input is high, the NMOS turns ON and the output becomes LOW.

The inverter is used inside the 6T SRAM cell and in supporting digital blocks.

### 2. 6T SRAM PUF Cell

The 6T SRAM cell is the main entropy source of the PUF.

It consists of two cross-coupled CMOS inverters, two access transistors, a wordline input called WL, and complementary bitlines called BL and BLB.

During power-up, transistor mismatch causes the SRAM cell to settle into either logic 0 or logic 1. This startup value is used as the raw PUF response bit.

The 6T SRAM cell provides the randomness needed to generate a unique device fingerprint.

### 3. Schmitt Trigger Sensing Circuit

The Schmitt Trigger is used to improve the reliability of the readout circuit.

It introduces hysteresis, which helps reject small voltage fluctuations around the switching threshold.

The Schmitt Trigger improves noise immunity, readout stability, digital transition reliability, robustness against weak SRAM startup states, and resistance to metastability-related uncertainty.

A normal inverter has one switching threshold, so noisy or weak input signals may cause unstable output. The Schmitt Trigger has hysteresis, making the output more stable.

### 4. 1-Bit Readout Circuit

The 1-bit readout circuit connects the SRAM cell to the Schmitt Trigger.

Its inputs are BL, BLB, WL, VDD, and GND. Its output is ReadoutBit.

The SRAM cell generates the startup state, and the Schmitt Trigger converts the sensed node voltage into a stable digital logic level.

This block improves the reliability of each generated PUF bit.

### 5. Word Line Decoder

The Word Line Decoder selects one row from the 4×4 SRAM array.

Its purpose is to activate only one row at a time, keep unselected rows disabled, allow controlled access to the SRAM cells, reduce unnecessary disturbance in the array, and support scalable row selection.

The decoder drives the selected wordline HIGH while keeping all other wordlines LOW.

### 6. Bit Detector

The Bit Detector is the final stage of the readout path.

Its role is to receive the ReadoutBit signal and make sure the bit is a valid digital logic value before it becomes part of the final PUF response.

The Bit Detector helps identify or handle unstable bits and improves the reliability of the final output.

Stable bit → Valid  
Unstable bit → Invalid

### 7. 4×4 SRAM PUF Array

The complete array contains 16 SRAM PUF cells arranged as 4 rows and 4 columns.

The cells share wordlines and bitline pairs. Each cell contributes one bit to the final 16-bit PUF response.

The array structure makes the design more scalable and organized.

## Tools Used

- Electric VLSI
- NGSpice
- CMOS full-custom design
- Hierarchical schematic design
- Layout implementation
- Transistor-level simulation

## Design Methodology

The project was implemented using a hierarchical design methodology.

Each block was designed and verified separately before being integrated into the final top-level system.

The design flow was:

CMOS Inverter → 6T SRAM Cell → Schmitt Trigger → 1-Bit Readout Circuit → Word Line Decoder → Bit Detector → 4×4 SRAM PUF Array → Top-Level Integrated System

This approach made the design easier to test, debug, and integrate.

## Simulation Setup

General simulation conditions:

| Parameter | Value |
|---|---|
| Design Tool | Electric VLSI |
| Simulation Tool | NGSpice |
| Supply Voltage | 5 V |
| Schmitt Trigger Input Sweep | 0 V to 5 V |
| Sweep Step | 0.05 V |
| Readout Load Capacitor | 20 fF |
| Readout WL | 5 V |

## Simulation Results

### Schmitt Trigger Results

The Schmitt Trigger was tested using a DC sweep from 0 V to 5 V.

| Input | Output | Logic |
|---|---|---|
| 0 V | ≈ 5.00 V | HIGH |
| 5 V | ≈ 0.043 V | LOW |

The switching threshold was approximately VTH ≈ 2.7 V.

These results confirm that the Schmitt Trigger produces full CMOS logic swing and can regenerate stable digital levels.

### 1-Bit Readout Results

The readout circuit was tested using complementary bitline conditions with WL held at 5 V.

| BL | BLB | WL | ReadoutBit | Logic |
|---|---|---|---|---|
| 5 V | 0 V | 5 V | ≈ 0.043 V | 0 |
| 0 V | 5 V | 5 V | ≈ 4.64 V | 1 |

The readout circuit successfully generated valid digital logic levels for complementary SRAM states.

## Baseline SRAM PUF vs Proposed Design

| Feature | Baseline SRAM PUF | Proposed Schmitt Trigger PUF |
|---|---|---|
| Entropy Source | SRAM mismatch | SRAM mismatch |
| Readout | Direct / inverter-based | Schmitt Trigger assisted |
| Noise Immunity | Lower | Higher |
| Output Stability | More sensitive to weak bits | Improved by hysteresis |
| Unstable Bit Detection | Not included | Added using Bit Detector |
| Area Cost | Lower | Slightly higher |
| CMOS Compatibility | Standard CMOS | Standard CMOS |
| PUF Suitability | Functional baseline | More reliable readout |

The proposed design improves noise immunity and output stability by adding the Schmitt Trigger sensing stage and Bit Detector.

## Layout Implementation

The layout was implemented using Electric VLSI.

The layout methodology followed standard CMOS full-custom design practices. PMOS devices were placed in the pull-up network, NMOS devices were placed in the pull-down network, and power and ground rails were routed consistently.

Hierarchical cells were used for easier integration. SRAM cell layout considered symmetry and matching, while Schmitt Trigger feedback routing was kept short and direct. DRC checking was considered for layout verification.

Implemented layout blocks include CMOS Inverter, 6T SRAM Cell, Schmitt Trigger, Readout Circuit, WL Decoder, Bit Detector, 4×4 SRAM PUF Array, and Top-Level Design.

## Repository Structure

Suggested repository structure:

Reliable-CMOS-PUF-Cell-for-IoT-Hardware-Security/
├── README.md
├── Project_IC.jelib
├── docs/
│   ├── Reliable_CMOS_PUF_Cell_for_IoT_Hardware_Security_Using_Schmitt_Trigger_Assisted_Readout.pdf
│   └── Reliable-CMOS-PUF-Cell-for-IoT-Hardware-Security.pdf
└── images/
    ├── inverter_schematic.png
    ├── inverter_layout.png
    ├── sram_cell_schematic.png
    ├── sram_cell_layout.png
    ├── schmitt_trigger_schematic.png
    ├── schmitt_trigger_layout.png
    ├── readout_circuit.png
    ├── bit_detector.png
    ├── array_4x4.png
    └── top_level_design.png

## Files Included

This repository may include the Electric VLSI project file, final project report, presentation slides, circuit screenshots, layout screenshots, simulation waveforms, and documentation files.

Main project files:

- Project_IC.jelib
- Reliable_CMOS_PUF_Cell_for_IoT_Hardware_Security_Using_Schmitt_Trigger_Assisted_Readout.pdf
- Reliable-CMOS-PUF-Cell-for-IoT-Hardware-Security.pdf

## Limitations

The current project verifies the main circuit blocks using schematic-level simulations.

Current limitations include no fabricated-chip measurements, no full layout-extracted simulation, no Monte Carlo mismatch simulation, no repeated power-up testing, no full bit error rate evaluation, and no complete PVT corner analysis.

## Future Work

Future improvements include layout-extracted simulation, Monte Carlo mismatch analysis, supply voltage sweep, temperature sweep, repeated power-up simulation, response balance calculation, bit error rate estimation, scaling the design to larger SRAM PUF arrays, and fabrication with silicon validation.

## Conclusion

This project demonstrates a compact Schmitt Trigger assisted SRAM PUF architecture for IoT hardware security.

The 6T SRAM cell provides the process-variation-based entropy source, while the Schmitt Trigger improves readout reliability by rejecting noise and stabilizing weak transitions.

The final system integrates the CMOS inverter, SRAM PUF cell, Schmitt Trigger, readout circuit, Word Line Decoder, Bit Detector, and 4×4 SRAM array into a complete hierarchical design.

The simulation results confirm that the proposed readout circuit produces valid digital output levels and improves the reliability of SRAM startup-state detection.

This makes the design suitable as a lightweight CMOS-based hardware fingerprint generator for IoT security applications.
