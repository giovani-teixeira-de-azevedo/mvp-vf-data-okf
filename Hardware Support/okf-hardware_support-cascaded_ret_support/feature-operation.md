---
type: concept
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#feature-operation
title: Feature Operation
description: Details the operational workflow of Cascaded RET Support, including AISG
  bus scanning, device discovery, calibration, electrical tilt configuration, and
  continuous supervision.
tags:
- aisg
- ret
- ald
- cascade
- calibration
- electrical-tilt
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-29T16:02:28+00:00'
  source_sha256: 0ebd889a6f4fa440
sources:
- title: Cascaded RET Support
  resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
---

This section details the operational workflow for Cascaded RET Support in the gNodeB, covering AISG bus scanning, device discovery and address assignment, actuator calibration, electrical tilt adjustment, and continuous device supervision.

## AISG Bus Scanning and Device Discovery

Operation begins with bus scanning. When a RET-capable port is unlocked, the node runs the AISG device-scan state machine:
- Broadcasts an XID address-assignment frame.
- Collects unique-ID responses from all unaddressed devices.
- Resolves collisions by mask refinement.
- Assigns each Antenna Line Device (ALD) a distinct HDLC address.

For each discovered device, the node reads the device type, vendor code, serial number, and number of subunits (multi-RET actuators expose one subunit per drivable array), and then instantiates or matches a `RetDevice` MO.

## Operation Sequence

```mermaid
sequenceDiagram 
    participant OSS 
    participant Node as gNodeB (AISG master) 
    participant ALD as Cascaded ALDs 
    OSS->>Node: unlock AISG port / action scanAisgBus 
    Node->>ALD: XID address assignment (broadcast, mask refinement) 
    ALD-->>Node: Unique IDs, HDLC addresses assigned 
    Node->>ALD: Get Device Type / Serial / Subunits 
    Node->>OSS: RetDevice MOs created (one per actuator) 
    OSS->>Node: action calibrate (RetDevice=2) 
    Node->>ALD: Calibrate subunit (full travel sweep) 
    ALD-->>Node: Calibration OK, tilt range reported 
    OSS->>Node: set electricalTilt=45 (0.1° units) 
    Node->>ALD: Set Tilt 
    ALD-->>Node: Tilt reached, read-back value stored
```

## Calibration, Tilt Control, and Supervision

After discovery, each actuator must be calibrated once (a full-travel sweep that lets the actuator learn its mechanical end stops) before tilt commands are accepted.

- **Electrical Tilt Control:** Tilt is set in 0.1-degree units within the device-reported range.
- **Continuous Supervision:** The node continuously supervises each device with a keep-alive poll.
- **Fault Localization:** A device that stops responding raises an ALD supervision alarm identifying the exact position in the cascade, which materially shortens fault localization on shared-antenna sites.
