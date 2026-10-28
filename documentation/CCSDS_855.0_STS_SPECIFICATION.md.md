# CCSDS 855.0-M-1 Subnetwork Test Service (STS) Engineering Specification

## 1\. Executive Summary & Purpose

The **Subnetwork Test Service (STS)** defined in **CCSDS 855.0-M-1 (Magenta Book, Issue 1, December 2009\)** provides a standardized, hardware-agnostic capability for spacecraft onboard flight software and test equipment to verify the integrity, reachability, and bit error performance of underlying subnetwork links.

STSForge Pro implements the full normative surface of CCSDS 855.0-M-1, enabling:

- Real-time round-trip latency (RTT) measurement with microsecond resolution.  
- Bit error rate (BER) qualification across SpaceWire (ECSS-E-ST-50-52C), MIL-STD-1553B, CANopen, and Ethernet subnetworks.  
- Autonomous onboard fault detection, isolation, and recovery (FDIR) support.  
- Non-intrusive diagnostic channel isolation preventing interference with flight-critical telemetry and telecommand flows.

---

## 2\. Architectural Context in SOIS

Within the **CCSDS Spacecraft Onboard Interface Services (SOIS)** reference architecture (CCSDS 850.0-G-1):

- **Layer**: SOIS Subnetwork Layer.  
- **Service Classification**: Diagnostic & Test Utility Service.  
- **Peer Services**:  
  - Subnetwork Packet Service (SPS — CCSDS 851.0-M-1)  
  - Subnetwork Memory Access Service (SMAS — CCSDS 852.0-M-1)  
  - Subnetwork Synchronisation Service (SSS — CCSDS 853.0-M-1)  
  - Subnetwork Device Discovery Service (SDDS — CCSDS 854.0-M-1)  
  - Device Access Service (DAS — CCSDS 871.0-M-1)  
  - Time Access Service (TAS — CCSDS 872.0-M-1)

---

## 3\. Normative Service Primitives & Parameters (§3.2)

### 3.1 TEST.request (§3.2.2)

Invoked by the user entity to transmit test data to a destination subnetwork entity and request an echo response.

- **Destination Subnetwork Address**: Target node address on the physical subnetwork.  
- **Channel**: Dedicated test channel or subchannel identifier.  
- **Priority**: Transmission priority level for scheduling across arbitration matrices.  
- **Test Data**: Arbitrary octet string or pseudorandom bit sequence (PRBS-9).

### 3.2 TEST.indication (§3.2.3)

Delivered to the user entity upon successful reception and verification of the echoed test response.

- **Source Subnetwork Address**: Responding entity address.  
- **Destination Subnetwork Address**: Original requesting entity address.  
- **Channel**: Diagnostic channel through which the test was completed.  
- **Priority**: Received priority level.  
- **Test Data**: The returned payload octets.  
- **Status**: Verification status (`SUCCESS` or `DATA_CORRUPTION`).  
- **Round-Trip Latency**: Elapsed duration in microseconds (μs).  
- **Bit Errors**: Quantitative count of corrupted bits detected against original pattern.

### 3.3 TEST\_FAILURE.indication (§3.2.4)

Delivered when a test request cannot be fulfilled due to network or hardware constraints.

- **Destination Subnetwork Address**: Target node.  
- **Channel**: Attempted test channel.  
- **Priority**: Attempted priority.  
- **Test Data**: Original test payload.  
- **Failure Reason**:  
  - `UNREACHABLE_DESTINATION`: Remote node did not acknowledge or route failure.  
  - `TEST_TIMEOUT`: Echo expired beyond predefined timing threshold.  
  - `LINK_DOWN`: Physical transceiver or link layer disconnected.  
  - `BUFFER_OVERFLOW`: Local or remote queue exhaustion.

---

## 4\. Subnetwork-Specific Mapping Details

### 4.1 SpaceWire (ECSS-E-ST-50-52C / CCSDS 855.0)

- **Protocol Identifier**: `0x03` (SOIS Subnetwork Test Service).  
- **Packet Structure**:  
  - `[0]` Target Logical Address (e.g., `0xFE` or `0x1A`)  
  - `[1]` Protocol ID (`0x03`)  
  - `[2]` Reserved / Channel Identifier (`0x0E`)  
  - `[3..N]` PRBS-9 or Test Payload  
  - `[N+1]` End of Packet (`EOP`) Normal Marker

### 4.2 MIL-STD-1553B

- **Subaddress**: Standard Subaddress 30 (Loopback Subaddress) or dedicated diagnostic subaddress.  
- **Operation**: The Bus Controller (BC) transmits 1 to 32 data words to Remote Terminal (RT) SA 30 (Receive), followed immediately by a Transmit command to SA 30 to read back and verify reflected words.

### 4.3 CAN / CANopen

- **Identifier**: Diagnostic COB-ID range (`0x7E0` \- `0x7E7`).  
- **Data Frame**: 8-byte payload carrying test sequence and incremental rolling index.