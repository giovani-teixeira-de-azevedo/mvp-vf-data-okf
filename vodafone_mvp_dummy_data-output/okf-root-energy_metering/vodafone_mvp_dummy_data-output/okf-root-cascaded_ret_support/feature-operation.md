---
type: concept
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#feature-operation
title: FEATURE OPERATION
description: Outlines the operational procedures for Cascaded RET Support, including
  AISG bus scanning, device discovery, calibration, electrical tilt adjustment, and
  continuous supervision.
tags:
- aisg
- ret
- ald
- gnodeb
- retdevice
- electrical-tilt
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T10:08:38+00:00'
  source_sha256: 0ebd889a6f4fa440
sources:
- title: Cascaded RET Support
  resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
---

This section details the operational workflow for the Cascaded RET Support feature, describing the sequence from AISG bus scanning and device discovery through calibration, electrical tilt control, and continuous device supervision.

## Bus Scanning and Device Discovery

Operation begins with bus scanning. When a RET-capable port is unlocked, the node runs the AISG device-scan state machine through the following process:

1. **Address Assignment**: The node broadcasts an XID address-assignment frame and collects unique-ID responses from all unaddressed devices.
2. **Collision Resolution**: Collisions are resolved using mask refinement, after which each ALD is assigned a distinct HDLC address.
3. **Device Identification**: For each discovered device, the node reads:
   - Device type
   - Vendor code
   - Serial number
   - Number of subunits (multi-RET actuators expose one subunit per drivable array)
4. **MO Instantiation**: The node instantiates or matches a `RetDevice` Managed Object (MO).

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

- **Calibration**: After discovery, each actuator must be calibrated once via a full-travel sweep (allowing the actuator to learn its mechanical end stops) before any tilt commands are accepted.
- **Electrical Tilt Adjustment**: Tilt is set in 0.1-degree units within the range reported by the device.
- **Continuous Supervision**: The node continuously supervises each device with a keep-alive poll. If a device stops responding, an ALD supervision alarm is raised identifying the exact position in the cascade, which materially shortens fault localization on shared-antenna sites.

# Cross-References

- [Feature Overview](feature-overview.md)
- [Activation Procedure](activation-procedure.md)
- [Parameters](parameters.md)
