---
type: concept
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#feature-operation
title: Feature Operation
description: Describes the operational workflow for Cascaded RET Support, including
  bus scanning, device discovery, calibration, tilt configuration, and device supervision.
tags:
- aisg
- ret
- ald
- cascaded-ret
- gnodeb
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T12:42:47+00:00'
  source_sha256: 0ebd889a6f4fa440
sources:
- resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
  title: Cascaded RET Support
---

This section details the operational flow of the Cascaded RET Support feature in the gNodeB (AISG master), covering bus scanning, device discovery, subunit calibration, electrical tilt configuration, and ongoing device supervision.

## Discovery and Scanning Workflow

Operation begins with bus scanning when a RET-capable port is unlocked. The node executes the AISG device-scan state machine:

1. **Address Assignment:** Broadcasts an XID address-assignment frame and collects unique-ID responses from all unaddressed devices.
2. **Collision Resolution:** Resolves collisions using mask refinement and assigns each Antenna Line Device (ALD) a distinct HDLC address.
3. **Device Information Retrieval:** Reads the device type, vendor code, serial number, and number of subunits for each discovered device. Multi-RET actuators expose one subunit per drivable array.
4. **MO Instantiation:** Instantiates or matches a `RetDevice` Managed Object (MO) for each discovered actuator.

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

## Calibration, Tilt Setting, and Supervision

- **Calibration:** Following discovery, each actuator must undergo a single calibration (a full-travel sweep allowing the actuator to learn its mechanical end stops) before any tilt commands are accepted.
- **Electrical Tilt Adjustment:** Tilt is configured in 0.1-degree units within the range reported by the device.
- **Continuous Supervision:** The node continuously supervises each device using a keep-alive poll. If a device stops responding, the node raises an ALD supervision alarm specifying the exact position of the fault within the cascade, accelerating fault localization on shared-antenna sites.

# Cross-References

- [Feature Overview](feature-overview.md)
