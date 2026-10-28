# STSForge Pro (v1.0.0)

## CCSDS 855.0-M-1 Subnetwork Test Service (STS) Reference Implementation & Diagnostic Suite

[![Standard](https://img.shields.io/badge/CCSDS-855.0--M--1-blue.svg)](https://ccsds.org/Pubs/855x0m1.pdf) [![SOIS Area](https://img.shields.io/badge/SOIS-Subnetwork_Test_Service-00b0ff.svg)](https://sanaregistry.org/) ![Compliance](https://img.shields.io/badge/Compliance-10%2F10_PASS-brightgreen.svg) ![License](https://img.shields.io/badge/License-Commercial_Flight_Lab_%2F_Dual_MIT-indigo.svg)

STSForge Pro is a production-grade implementation, simulation engine, and verification harness for the **Consultative Committee for Space Data Systems (CCSDS) 855.0-M-1** standard: *"Spacecraft Onboard Interface Services (SOIS) — Subnetwork Test Service"*.

It provides satellite flight software engineers, onboard avionics test bench teams, and spacecraft AIT (Assembly, Integration, and Test) specialists with a unified framework to benchmark round-trip latency (μs), measure bit error rates (BER), detect network degradation, and qualify SpaceWire, MIL-STD-1553B, CANopen, and Ethernet links.

---

## 1\. Key Capabilities

- **Full Normative Primitives Support (§3.2)**:  
  - `TEST.request`: Parameterized invocation supporting target subnetwork addresses, channels, priority levels, and arbitrary test payloads.  
  - `TEST.indication`: Accurate confirmation delivering reflected payloads, high-resolution RTT latency, and bit error calculations.  
  - `TEST_FAILURE.indication`: Autonomous error classification identifying link drops, target timeouts, and unreachable routes.  
- **Hardware-Grade PRBS-9 Generator**: Built-in 511-bit linear feedback shift register (x⁹ \+ x⁵ \+ 1\) for high-throughput bit error rate (BER) qualification.  
- **Protocol Encoders & Loopback Drivers**:  
  - SpaceWire (ECSS-E-ST-50-52C) logical addressing and SOIS Protocol Identifier `0x03`.  
  - MIL-STD-1553B Subaddress 30 loopback reflection data words.  
  - CANopen diagnostic echo frames (`COB-ID 0x7E0`).  
- **Glass-Cockpit Diagnostics Studio**: Interactive web application featuring live ping telemetry, PRBS-9 BER stress testing, and real-time normative audit reporting.  
- **Headless Automation CLI**: Python 3 command-line utility for CI/CD test bench integration and automated spacecraft qualification loops.

---

## 2\. Directory Structure

sts-forge-pro/

├── source-code/

│   ├── sts-engine.js           \# Core ES6 SOIS STS state machine and PRBS-9 generator

│   ├── sts-auditor.js          \# 10-point normative compliance audit engine

│   ├── sts\_cli.py              \# Python 3 CLI suite for avionics test benches

│   ├── test\_runner.py          \# Automated verification test suite (10/10 PASS)

│   └── index.html              \# Glass-cockpit web diagnostics studio

├── documentation/

│   ├── CCSDS\_855.0\_STS\_SPECIFICATION.md  \# Detailed technical specification

│   └── STS\_COMPLIANCE\_MATRIX.md         \# 10-point clause verification matrix

├── screenshots/

│   ├── sts-architecture-diagram.svg     \# Full-stack vector architecture diagram

│   └── sts-studio-preview.svg            \# Diagnostics workstation interface mockup

├── build/

│   └── sts-forge-pro-v1.0.0.zip          \# Production distribution package

└── README.md                            \# Comprehensive product manual

---

## 3\. Quickstart Guide

### 3.1 Running the Automated Test Suite

Verify that all 10 unit test cases pass:

python3 source-code/test\_runner.py

### 3.2 Running the Avionics Test Bench CLI

List registered subnetwork endpoints:

python3 source-code/sts\_cli.py list-nodes

Ping a SpaceWire endpoint (`0x1A`) over diagnostic channel 14:

python3 source-code/sts\_cli.py ping-node \--dest 0x1A \--channel 14 \--pattern PRBS9

Run a 1,000-frame Bit Error Rate (BER) stress test:

python3 source-code/sts\_cli.py ber-test \--dest 0x1A \--frames 1000 \--channel 14

Dissect a raw SpaceWire frame to inspect SOIS STS Protocol ID `0x03`:

python3 source-code/sts\_cli.py dissect-frame \--frame "1A030E55AA"

Execute the full CCSDS 855.0 normative audit:

python3 source-code/sts\_cli.py audit

### 3.3 Launching the Glass-Cockpit Web Studio

Open `source-code/index.html` in any modern web browser.

---

## 4\. Monetization & Licensing

- **Commercial Flight Lab License**: \$3,450 / seat perpetual license with 12 months of standard updates and test bench integration support.  
- **Dual MIT / Educational License**: Open-core simulator available for university CubeSat teams and non-commercial aerospace research.

---

## 5\. Authoritative Standards & References

- **CCSDS 855.0-M-1**: [Spacecraft Onboard Interface Services — Subnetwork Test Service](https://ccsds.org/Pubs/855x0m1.pdf) (Magenta Book, December 2009).  
- **CCSDS 850.0-G-1**: Spacecraft Onboard Interface Services — An Overview (Green Book).  
- **ECSS-E-ST-50-52C**: SpaceWire — Remote Memory Access Protocol (RMAP).  
- **MIL-STD-1553B**: Digital Time Division Command/Response Multiplex Data Bus.