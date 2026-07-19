# Reliable CMOS PUF Cell for IoT Hardware Security

## Overview

This project presents a reliable CMOS SRAM-based Physically Unclonable Function (PUF) for lightweight IoT hardware security. The design uses a conventional 6T SRAM cell as the entropy source and integrates a Schmitt Trigger assisted readout circuit to improve sensing reliability, noise immunity, and output stability.

The complete architecture was implemented hierarchically using Electric VLSI and verified through NGSpice simulations. The system includes a CMOS inverter, 6T SRAM PUF cell, Schmitt Trigger sensing circuit, 1-bit readout circuit, Word Line Decoder, Bit Detector, 4x4 SRAM PUF array, and top-level integrated design.

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

IoT devices require lightweight, low-cost, and secure authentication methods. Traditional security systems often store secret keys in non-volatile memory, which can expose the device to physical attacks such as probing, reverse engineering, and memory extraction.

Physically Unclonable Functions (PUFs) provide an alternative approach by generating unique hardware fingerprints from natural CMOS manufacturing variations instead of storing permanent secret keys.

In SRAM-based PUFs, each SRAM cell powers up to either logic 0 or logic 1 depending on transistor mismatch. This startup value can be used as a unique response bit for device authentication or cryptographic key generation.

However, some SRAM cells may produce unstable responses due to noise, supply voltage variation, temperature changes, or metastability. To improve reliability, this project adds a Schmitt Trigger assisted readout stage to stabilize the sensed output.

## What is a PUF?

A Physically Unclonable Function (PUF) is a hardware security primitive that generates a unique response based on physical manufacturing variations.

Even if two chips are designed using the same schematic and layout, small manufacturing differences make each chip behave slightly differently. These differences create a unique hardware fingerprint that is extremely difficult to duplicate.

In this project, the startup behavior of SRAM cells is used to generate the PUF response.

## Why SRAM PUF?

SRAM PUFs are suitable for IoT security because:

- SRAM cells naturally exist in many digital systems.
- No permanent secret key needs to be stored.
- The response is generated from intrinsic transistor mismatch.
- The design is compact and CMOS-compatible.
- It is suitable for lightweight hardware authentication.
- It can be used for device identification and key generation.

## System Architecture

The proposed architecture follows this signal flow:

Address -> Word Line Decoder -> 4x4 SRAM PUF Array -> BL/BLB Bitlines -> Schmitt Trigger Assisted Readout -> Bit Detector -> PUF Output

The system generates a 16-bit PUF response using a 4x4 SRAM array. Each SRAM cell contributes one response bit. The Word Line Decoder selects the target row, the SRAM cell produces the startup state, the Schmitt Trigger stabilizes the sensed value, and the Bit Detector validates the final digital response bit.

## Main Circuit Blocks

### 1. CMOS Inverter

The CMOS inverter is the fundamental building block of the design.

It consists of one PMOS transistor in the pull-up network and one NMOS transistor in the pull-down network.

When the input is LOW, the PMOS turns ON and the output becomes HIGH. When the input is HIGH, the NMOS turns ON and the output becomes LOW.

The inverter is used inside the SRAM cell and other supporting digital blocks.

### 2. 6T SRAM PUF Cell

The 6T SRAM cell is the main entropy source of the PUF.

It consists of two cross-coupled CMOS inverters, two access transistors, wordline input WL, and complementary bitlines BL and BLB.

During power-up, transistor mismatch causes the SRAM cell to settle into either logic 0 or logic 1. This startup value is used as the raw PUF response bit.

The cross-coupled inverter structure provides bistability, while transistor mismatch gives each cell a unique preferred startup state.

### 3. Schmitt Trigger Sensing Circuit

The Schmitt Trigger is used to improve the reliability of the readout stage.

A conventional inverter has a single switching threshold, which makes it sensitive to weak or noisy input signals. In contrast, the Schmitt Trigger introduces hysteresis, allowing it to reject small voltage fluctuations around the switching point.

The Schmitt Trigger improves noise immunity, readout stability, digital transition reliability, robustness against weak SRAM startup states, and resistance to metastability-related uncertainty.

This makes the sensed PUF response more stable and reliable.

### 4. 1-Bit Readout Circuit

The 1-bit readout circuit connects the 6T SRAM PUF cell to the Schmitt Trigger sensing stage.

Inputs:

- BL
- BLB
- WL
- VDD
- GND

Output:

- ReadoutBit

The SRAM cell generates the startup state, and the Schmitt Trigger converts the sensed voltage into a stable digital logic level. This block improves the reliability of each generated PUF response bit.

### 5. Word Line Decoder

The Word Line Decoder selects one row from the 4x4 SRAM PUF array.

Its purpose is to activate only one row at a time, keep unselected rows disabled, control access to SRAM cells, reduce unwanted disturbance in the array, and support scalable array operation.

The decoder drives the selected wordline HIGH while keeping all other wordlines LOW.

### 6. Bit Detector

The Bit Detector is the final stage of the readout path.

It receives the ReadoutBit signal and checks whether the bit is a valid digital logic value before it becomes part of the final PUF output.

The Bit Detector improves the reliability and consistency of the generated hardware fingerprint.

Stable bit -> valid response bit  
Unstable bit -> detected or excluded  
Final output -> more reliable PUF response

### 7. 4x4 SRAM PUF Array

The complete PUF array contains sixteen 6T SRAM cells arranged as four rows and four columns.

Features:

- 16 SRAM PUF cells
- 4 rows and 4 columns
- Shared wordlines
- Shared bitline pairs
- One response bit generated per cell
- 16-bit total PUF response

The regular array structure makes the design scalable and suitable for larger PUF implementations.

## Tools Used

- Electric VLSI
- NGSpice
- CMOS full-custom design
- Hierarchical schematic design
- Layout implementation
- Transistor-level simulation

## Design Methodology

The project was implemented using a hierarchical design methodology. Each block was designed, tested, and verified separately before being integrated into the complete top-level system.

Design flow:

CMOS Inverter -> 6T SRAM PUF Cell -> Schmitt Trigger -> 1-Bit Readout Circuit -> Word Line Decoder -> Bit Detector -> 4x4 SRAM PUF Array -> Top-Level Integrated System

This methodology made the design easier to debug, verify, and integrate.

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
| 0 V | approximately 5.00 V | HIGH |
| 5 V | approximately 0.043 V | LOW |

The switching threshold was approximately VTH = 2.7 V.

These results confirm that the Schmitt Trigger produces a full CMOS logic swing and regenerates stable digital levels.

### 1-Bit Readout Results

The 1-bit readout circuit was tested using complementary bitline conditions with WL = 5 V.

| BL | BLB | WL | ReadoutBit | Logic |
|---|---|---|---|---|
| 5 V | 0 V | 5 V | approximately 0.043 V | 0 |
| 0 V | 5 V | 5 V | approximately 4.64 V | 1 |

The readout circuit successfully generated valid digital logic levels for complementary SRAM states. The output values are close to the supply rails, confirming stable response-bit generation.

## Baseline SRAM PUF vs Proposed Design

| Feature | Baseline SRAM PUF | Proposed Schmitt Trigger PUF |
|---|---|---|
| Entropy Source | SRAM mismatch | SRAM mismatch |
| Readout | Direct / inverter-based | Schmitt Trigger assisted |
| Noise Immunity | Lower | Higher |
| Output Stability | More sensitive to weak bits | Improved by hysteresis |
| Unstable Bit Handling | Limited | Improved using Bit Detector |
| Area Cost | Lower | Slightly higher |
| CMOS Compatibility | Standard CMOS | Standard CMOS |
| PUF Suitability | Functional baseline | More reliable readout |

The proposed design improves readout robustness by adding a Schmitt Trigger sensing stage and Bit Detector while maintaining standard CMOS compatibility.

## Layout Implementation

The layout was implemented using Electric VLSI following standard CMOS full-custom layout practices.

Layout considerations:

- PMOS devices placed in the pull-up network
- NMOS devices placed in the pull-down network
- Consistent VDD and GND routing
- Hierarchical cells used for easier integration
- SRAM cell layout designed with attention to matching and symmetry
- Schmitt Trigger feedback routing kept short and direct
- DRC checking considered for layout verification

Implemented layout blocks:

- CMOS Inverter
- 6T SRAM Cell
- Schmitt Trigger
- Readout Circuit
- Word Line Decoder
- Bit Detector
- 4x4 SRAM PUF Array
- Top-Level Design

## Repository Structure

Suggested repository structure:

Reliable-CMOS-PUF-Cell-for-IoT-Hardware-Security/
- README.md
- Project_IC.jelib
- docs/
  - Reliable_CMOS_PUF_Cell_for_IoT_Hardware_Security_Using_Schmitt_Trigger_Assisted_Readout.pdf
  - Reliable-CMOS-PUF-Cell-for-IoT-Hardware-Security.pdf
- images/
  - inverter_schematic.png
  - inverter_layout.png
  - sram_cell_schematic.png
  - sram_cell_layout.png
  - schmitt_trigger_schematic.png
  - schmitt_trigger_layout.png
  - readout_circuit.png
  - bit_detector.png
  - array_4x4.png
  - top_level_design.png

## Files Included

This repository may include:

- Electric VLSI project file
- Final project report
- Presentation slides
- Circuit screenshots
- Layout screenshots
- Simulation waveforms
- Documentation files

Main project files:

- Project_IC.jelib
- Reliable_CMOS_PUF_Cell_for_IoT_Hardware_Security_Using_Schmitt_Trigger_Assisted_Readout.pdf
- Reliable-CMOS-PUF-Cell-for-IoT-Hardware-Security.pdf

## Limitations

The current project verifies the main circuit blocks using schematic-level simulations.

Current limitations:

- No fabricated-chip measurements
- Layout-extracted simulation still needed
- Monte Carlo mismatch simulation still needed
- Repeated power-up testing still needed
- Full bit error rate evaluation still needed
- Complete PVT corner analysis still needed

## Future Work

Future improvements include:

- Layout-extracted simulation
- Monte Carlo mismatch analysis
- Supply voltage sweep
- Temperature sweep
- Repeated power-up simulation
- Response balance calculation
- Bit error rate estimation
- Scaling the design to larger SRAM PUF arrays
- Fabrication and silicon validation

## Conclusion

This project demonstrates a compact Schmitt Trigger assisted SRAM PUF architecture for IoT hardware security.

The 6T SRAM cell provides the process-variation-based entropy source, while the Schmitt Trigger improves readout reliability by rejecting noise and stabilizing weak transitions.

The final system integrates the CMOS inverter, SRAM PUF cell, Schmitt Trigger, 1-bit readout circuit, Word Line Decoder, Bit Detector, and 4x4 SRAM array into a complete hierarchical design.

Simulation results confirm that the proposed readout circuit produces valid digital output levels and improves the reliability of SRAM startup-state detection.

This makes the design suitable as a lightweight CMOS-based hardware fingerprint generator for IoT security applications.

## Keywords

CMOS, SRAM PUF, Physically Unclonable Function, Schmitt Trigger, Hardware Security, IoT Security, Electric VLSI, NGSpice, 6T SRAM, Readout Circuit
