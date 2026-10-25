# SENTForge Pro (v1.0.0)

### SAE J2716 APR2016 (SENT) & Short PWM Sensor Protocol Studio & 10-Point Normative Auditor

[![Standard](https://img.shields.io/badge/Standard-SAE%20J2716%20APR2016-blue.svg)](https://www.sae.org) [![CRC Engine](https://img.shields.io/badge/CRC--4-Appendix%20B%20Poly%20x^4%2Bx^3%2Bx^2%2B1-orange.svg)](#) [![Normative Auditor](https://img.shields.io/badge/Auditor-10--Point%20Normative%20PASS-brightgreen.svg)](#) [![License](https://img.shields.io/badge/License-Commercial%20B2B-purple.svg)](#)

---

## 1\. Executive Summary

**SENTForge Pro** is an automotive-grade engineering studio, codec, and normative verification auditor for the **SAE J2716 (Single Edge Nibble Transmission \- SENT)** and **Short PWM Codec (SPC)** automotive sensor standards.

Modern electric vehicles, ADAS sensor suites, electronic throttle bodies, brake-by-wire, and steer-by-wire systems require high-resolution, low-latency, and noise-immune sensor telemetry. Traditional 0–5V analog ratiometric signaling suffers from ground offset shifts, EMI degradation, and harness line losses, necessitating expensive ADCs and complex calibration routines in the Electronic Control Unit (ECU).

**SENT solves these limitations** by transmitting high-precision digital data across a single signal wire via falling-edge time interval measurements. SENTForge Pro provides an interactive single-page web workspace and a zero-dependency Python CLI runner that enables automotive sensor application engineers, ECU firmware developers, and validation test architects to:

1. **Synthesize & Visualize SENT Frames**: Configure clock ticks (\$1.0,\\mu\\text{s}\$ to \$90.0,\\mu\\text{s}\$), status nibbles, dual 12-bit, single 16/8-bit, or secure sensor profiles, and optional pause pulses with fixed-frame-period regulation (e.g. constant 282 ticks).  
2. **Compute Bit-Exact CRC-4**: Hardware-accurate table lookup conforming to SAE J2716 Appendix B (\$x^4 \+ x^3 \+ x^2 \+ 1\$, seed `0x5`).  
3. **Dissect Oscilloscope Pulse Streams**: Ingest raw tick or microsecond time intervals, calculate clock drift against the 56-tick calibration pulse, and extract fast channel sensor readings.  
4. **Multiplex Slow Serial Messages**: Model 16-frame Short Serial Message bitstreams across status nibble bits 2 and 3 for low-bandwidth diagnostics and sensor calibration.  
5. **Enforce 10-Point Normative Conformance**: Verify protocol compliance against 10 strict SAE J2716 validation assertions.

---

## 2\. Key Capabilities & Features

- **Multi-Profile Fast Channel Codec**:  
  - `dual_12_12`: Throttle Position (Main \+ Complementary), dual Hall-effect, or differential pressure.  
  - `single_16_8`: High-precision measurement (16 bits) \+ temperature/auxiliary telemetry (8 bits).  
  - `single_12`: Standard single-channel sensor.  
  - `secure_sensor`: 12-bit primary sensor \+ 8-bit rolling counter \+ 4-bit inverted security nibble.  
- **Timing & Pulse Engine**:  
  - 56-tick synchronization / calibration pulse verification with \$\\pm 20%\$ drift tolerance.  
  - Nibble period calculation: \$(12 \+ \\text{Nibble}) \\times t\_{\\text{tick}}\$ (\$12\\dots 27,\\text{ticks}\$).  
  - Variable pause pulse or constant frame tick locking (e.g., 282 ticks for constant sample rate).  
- **10-Point Normative Zero-Trust Compliance Auditor**:  
  - `RULE-SENT-01`: Clock Tick Unit Time Range Compliance (\$1.0,\\mu\\text{s} \\le t\_{\\text{tick}} \\le 90.0,\\mu\\text{s}\$).  
  - `RULE-SENT-02`: Calibration / Sync Pulse Ratio (\$56,\\text{ticks} \\pm 20%\$).  
  - `RULE-SENT-03`: Nibble Pulse Width Bounds (\$12 \\le \\text{Ticks} \\le 27\$).  
  - `RULE-SENT-04`: Status & Communication Nibble Semantic Validity.  
  - `RULE-SENT-05`: Fast Channel Profile Bit-Depth Boundary Compliance.  
  - `RULE-SENT-06`: SAE J2716 Appendix B CRC-4 Mathematical Correctness.  
  - `RULE-SENT-07`: Minimum Low Period Verification (\$\\ge 4,\\text{ticks}\$ or \$\\ge 5.0,\\mu\\text{s}\$).  
  - `RULE-SENT-08`: Pause Pulse Minimum Width Compliance (\$\\ge 12,\\text{ticks}\$ when present).  
  - `RULE-SENT-09`: Total Frame Period Bounds (\$84\\dots 500,\\text{ticks}\$).  
  - `RULE-SENT-10`: Slow Serial Message Framing Protocol (Bit 3 Start marker validation).  
- **Interactive Oscilloscope Waveform**: Real-time colored visualizer displaying sync, status, data nibbles, CRC-4, and pause pulses with interactive tooltips.  
- **Standalone Zero-Dependency Architecture**: Runs completely in modern web browsers offline and provides a Python 3 CLI runner (`build/sent-cli.py`).

---

## 3\. Directory Layout

2026-10-11 — SENTForge Pro/

├── README.md                                \# Root product specification

├── build/

│   ├── sent-cli.py                         \# Standalone Python CLI tool

│   └── sent-pro-v1.0.0.zip                 \# Release distribution archive

├── documentation/

│   ├── ARCHITECTURE.md                     \# SAE J2716 protocol timing & CRC-4 math

│   ├── USER\_GUIDE.md                       \# Web studio & CLI operator guide

│   └── MONETIZATION\_AND\_MARKETING\_PLAYBOOK.md \# Commercialization & GTM strategy

├── screenshots/

│   └── sent-studio-preview.svg             \# UI vector screenshot

└── source-code/

    ├── index.html                          \# Engineering web studio application

    ├── sent.css                            \# Automotive dark-mode stylesheet

    ├── sent-engine.js                      \# Core SAE J2716 codec & CRC engine

    ├── sent-auditor.js                     \# 10-point normative compliance ruleset

    └── app.js                              \# UI interactive application controller

---

## 4\. Quickstart Guide

### Web Application

Open `source-code/index.html` in any modern web browser. No server or Internet connection required.

### CLI Usage (Python 3.8+)

\# Synthesize a standard dual 12-bit throttle frame

python3 build/sent-cli.py frame \--tick 3.0 \--profile dual\_12\_12 \--d1 3120 \--d2 975 \--fixed-ticks 282

\# Run 10-point normative compliance audit

python3 build/sent-cli.py audit \--tick 3.0 \--d1 2048 \--d2 1024

\# Generate 16-frame slow serial telemetry sequence

python3 build/sent-cli.py serial \--msg-id 1 \--data 165

### Running Automated Test Suite

python3 test\_sent.py

Expected output:

\============================================================

SENTForge Pro \- SAE J2716 Automated Verification Suite

\============================================================

\[PASS\] TEST 1: CRC-4 Calculation (Result: 0x0)

\[PASS\] TEST 2: Pulse Tick Conversion (0x0=12t, 0xF=27t)

\[PASS\] TEST 3: Frame Synthesis (Dual 12-bit, Total Ticks: 209\)

\[PASS\] TEST 4: Fixed Frame Period Extension (Total Ticks: 282\)

\[PASS\] TEST 5: High-Precision 16-bit / 8-bit Profile Synthesis

\[PASS\] TEST 6: Normative Auditor \- Compliant Frame (10/10 PASS)

\[PASS\] TEST 7: Clock Tick Violation Correctly Flagged (\> 90.0 us)

\[PASS\] TEST 8: Fault Injection \- Corrupted CRC-4 Correctly Detected

\[PASS\] TEST 9: Slow Serial Message Encoding (ID: 0x2, Data: 0x4B, CRC: 0xB)

\[PASS\] TEST 10: CLI Execution & JSON Serialization

\============================================================

RESULTS: 10 Passed, 0 Failed (100.0% Pass Rate)

\============================================================

---

## 5\. Commercial Licensing & Target Market

- **Target Customers**: Automotive Sensor Tier-1 Suppliers (Bosch, Continental, Denso, Sensata, Melexis, Allegro, Infineon, NXP), Automotive OEMs (BMW, Mercedes-Benz, Porsche, GM, Ford, Tesla, BYD), and Test Bench Engineers.  
- **Pricing & Packaging**:  
  - **Single Engineer Desktop License**: \$1,290 / seat / year  
  - **Tier-1 Team Site License (Floating 10 seats)**: \$7,490 / year  
  - **OEM Enterprise Core (HIL Automation & Escrow)**: \$19,500 one-time perpetual license

---

*SENTForge Pro is developed and certified by the Autonomous Digital Product Factory.*