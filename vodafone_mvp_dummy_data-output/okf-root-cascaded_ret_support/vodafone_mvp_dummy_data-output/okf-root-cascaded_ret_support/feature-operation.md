---
type: concept
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#feature-operation
title: Feature Operation
description: Describes the operational workflow, AISG bus scanning, device discovery,
  calibration, electrical tilt configuration, and continuous supervision for Cascaded
  RET.
tags:
- aisg
- ret
- bus-scanning
- calibration
- electrical-tilt
- ald-supervision
- gnodeb
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T07:59:46+00:00'
  source_sha256: 0ebd889a6f4fa440
sources:
- title: Cascaded RET Support
  resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
---

This section details the operational workflow for Cascaded RET Support, including AISG bus scanning, ALD device discovery, subunit calibration, electrical tilt configuration, and continuous keep-alive supervision.

## Operational Workflow

### Bus Scanning and Device Discovery

Operation begins with bus scanning. When a RET-capable port is unlocked, the node runs the AISG device-scan state machine:
- Broadcasts an XID address-assignment frame.
- Collects unique-ID responses from all unaddressed devices.
- Resolves collisions by mask refinement.
- Assigns each Antenna Line Device (ALD) a distinct HDLC address.

For each discovered device, the node reads the device type, vendor code, serial number, and number of subunits (multi-RET actuators expose one subunit per drivable array), then instantiates or matches a `RetDevice` Managed Object (MO).

### Message Flow

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

### Calibration, Tilt Control, and Supervision

- **Calibration:** After discovery, each actuator must be calibrated once via a full-travel sweep that lets the actuator learn its mechanical end stops before tilt commands are accepted.
- **Tilt Control:** Tilt is set in 0.1-degree units within the device-reported range.
- **Continuous Supervision:** The node continuously supervises each device with a keep-alive poll. A device that stops responding raises an ALD supervision alarm identifying the exact position in the cascade, shortening fault localization on shared-antenna sites.

## Cross-References

- [Feature Overview](feature-overview.md)
- [Parameters](parameters.md)
