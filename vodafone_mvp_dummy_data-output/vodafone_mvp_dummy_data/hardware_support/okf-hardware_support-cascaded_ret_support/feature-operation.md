---
type: concept
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#feature-operation
title: Feature Operation
description: Explains the operation of cascaded RET support, including AISG bus scanning,
  ALD device discovery, calibration, electrical tilt control, and continuous device
  supervision.
tags:
- aisg
- ret
- ald
- hdlc
- calibration
- tilt
- gnodeb
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T15:10:30+00:00'
  source_sha256: 0ebd889a6f4fa440
sources:
- title: Cascaded RET Support
  resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
---

This section describes the operational workflow for Cascaded RET Support, including AISG bus scanning, Antenna Line Device (ALD) discovery, address assignment, subunit calibration, electrical tilt configuration, and continuous device supervision by the gNodeB.

## Bus Scanning and Device Discovery

Operation begins with bus scanning. When a RET-capable port is unlocked, the node runs the AISG device-scan state machine:

1. **Address Assignment**: The node broadcasts an XID address-assignment frame, collects unique-ID responses from all unaddressed devices, resolves collisions by mask refinement, and assigns each ALD a distinct HDLC address.
2. **Device Data Reading**: For each discovered device, the node reads the device type, vendor code, serial number, and number of subunits (multi-RET actuators expose one subunit per drivable array).
3. **MO Instantiation**: The node instantiates or matches a `RetDevice` Managed Object (MO).

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

* **Calibration**: After discovery, each actuator must be calibrated once via a full-travel sweep—allowing the actuator to learn its mechanical end stops—before tilt commands are accepted.
* **Electrical Tilt Setting**: Tilt is set in 0.1-degree units within the device-reported range.
* **Supervision and Alarms**: The node continuously supervises each device with a keep-alive poll. If a device stops responding, an ALD supervision alarm is raised identifying the exact position in the cascade, which materially shortens fault localization on shared-antenna sites.
