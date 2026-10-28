Record \#508

# STSForge Pro (v1.0.0)

Spacecraft Onboard Interface Services (SOIS) — Subnetwork Test Service Studio (CCSDS 855.0-M-1)  
MAGENTA BOOK COMPLIANT

Active Nodes

4

RTT Latency (Avg)

142 μs

Bit Error Rate (BER)

0.00e+00

Audit Score

100%

## 

Target Node Selector

---

Destination Subnetwork Node 0x1A \- Payload Mass Memory (SpaceWire) 0x22 \- Star Tracker A (MIL-STD-1553B) 0x34 \- Power Dist. Unit (CANopen) 0xFE \- Degraded Gyro Link (Fault Demo) Test Data Pattern PRBS-9 Pseudorandom (x⁹ \+ x⁵ \+ 1\) Alternating Stress Pattern (0xAA55) Custom Hex String Dedicated Diagnostic Channel Execute TEST.request (Ping) Run 1K-Frame BER Stress Test Run CCSDS 855.0 Normative Audit

## 

Subnetwork Node Topology

* ---

  \[0x1A\] Mass Memory (SpW)READY  
* \[0x22\] Star Tracker (1553B)READY  
* \[0x34\] PDU (CANopen)READY  
* \[0xFE\] Degraded Link (Fault)BER 0.05

## 

Subnetwork Diagnostic Console (SOIS Primitives Log)

---

\[INIT\] STSForge Pro v1.0.0 Initialized. \[SOIS\] CCSDS 855.0-M-1 Subnetwork Test Service Active. \[SUBNET\] Registered 4 Onboard Test Endpoints (SpaceWire, MIL-STD-1553B, CANopen). \[STATUS\] Ready to execute TEST.request primitives.

## 

Normative Compliance Audit Matrix (CCSDS 855.0-M-1)

---

| Req ID | Clause | Requirement Description | Verified Primitive / Logic | Verdict   |
| :---- | :---- | :---- | :---- | :---- |
| STS-001 | §3.2.2.1 | TEST.request primitive invocation | DestAddr, Channel, Priority, Data | PASS |
| STS-002 | §3.2.3.1 | TEST.indication primitive delivery | Round-Trip Latency & Bit Error checks | PASS |
| STS-003 | §3.2.4.1 | TEST\_FAILURE.indication on link loss | UNREACHABLE\_DESTINATION code | PASS |
| STS-004 | §3.1.3 | Diagnostic test channel isolation | Dedicated channel 14 validation | PASS |
| STS-005 | §2.3.1 | Loopback echo payload preservation | Bitwise data identity verification | PASS |

