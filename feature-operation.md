---
type: concept
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#feature-operation
title: FEATURE OPERATION
description: Describes the operational process for Cascaded RET Support, including
  AISG bus scanning, ALD discovery, device calibration, electrical tilt control, and
  continuous supervision.
tags:
- aisg
- ald
- ret
- bus-scanning
- calibration
- electrical-tilt
- gnodeb
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T18:52:42+00:00'
  source_sha256: 0ebd889a6f4fa440
sources:
- resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
  title: Cascaded RET Support
---

This section details the operational workflow for Cascaded RET Support in the gNodeB, covering AISG bus scanning, device discovery and Managed Object (MO) creation, actuator calibration, electrical tilt adjustment, and continuous supervision.

## AISG Bus Scanning and Device Discovery

Operation begins with bus scanning when a RET-capable port is unlocked. The node runs the AISG device-scan state machine:

1. Broadcasts an XID address-assignment frame.
2. Collects unique-ID responses from all unaddressed devices.
3. Resolves collisions by mask refinement.
4. Assigns each ALD a distinct HDLC address.

For each discovered device, the node reads:
- Device type
- Vendor code
- Serial number
- Number of subunits (multi-RET actuators expose one subunit per drivable array)

Following discovery, the node instantiates or matches a `RetDevice` MO for each actuator.

## Sequence Diagram

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

- **Calibration**: After discovery, each actuator must be calibrated once through a full-travel sweep (allowing the actuator to learn its mechanical end stops) before tilt commands are accepted.
- **Tilt Control**: Tilt is set in 0.1-degree units within the device-reported range.
- **Continuous Supervision**: The node continuously supervises each device using a keep-alive poll. If a device stops responding, an ALD supervision alarm is raised that identifies the exact position in the cascade, which materially shortens fault localization on shared-antenna sites.

# Cross-References

- [FEATURE OVERVIEW](feature-overview.md)
- [PARAMETERS](parameters.md)
- [ACTIVATION PROCEDURE](activation-procedure.md)
