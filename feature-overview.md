---
type: concept
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#feature-overview
title: FEATURE OVERVIEW
description: Overview of Cascaded RET Support extending node Remote Electrical Tilt
  control to daisy-chained AISG devices.
tags:
- RET
- AISG
- ALD
- Remote Electrical Tilt
- Radio Unit
- O&M
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T18:52:46+00:00'
  source_sha256: 5b0eb4d97409130e
sources:
- resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
  title: Cascaded RET Support
---

This section provides an overview of Cascaded RET Support, extending Remote Electrical Tilt (RET) control to daisy-chained Antenna Interface Standards Group (AISG) devices on a single bus.

## Overview and Topology

Cascaded RET Support extends the node's Remote Electrical Tilt (RET) control capability to antenna installations where multiple RET motor units are daisy-chained on a single AISG bus, rather than each RET unit having its own dedicated control port.

This is the dominant physical topology on multi-band macro sites: a single antenna radome frequently houses two to six independently tiltable arrays (for example, a low-band array plus two mid-band arrays). Site installers connect all of their RET actuators in a cascade on one AISG control cable to minimize feeder and connector count.

Without cascade support, the node can only address the first RET unit on the bus; the remaining actuators are invisible to O&M and can only be tilted by a technician with a portable controller at the site, defeating the purpose of remote electrical tilt.

## Device Discovery and Management

With this feature activated:
- The node performs AISG v2.0/v3.0 device scanning on each RET-capable port.
- Discovers every Antenna Line Device (ALD) on the chain via its unique HDLC address assignment procedure.
- Creates one manageable `RetDevice` MO instance per discovered actuator.

Each actuator can then be calibrated, tilted, and supervised individually from the OSS, exactly as if it were connected point-to-point.

## Transport Variants

The AISG bus is carried either on:
- A dedicated multi-core control cable (RS-485 layer)
- Modulated onto the antenna feeder as an On-Off Keying (OOK) signal injected by a smart bias-tee in the radio unit

Both transport variants are supported, and mixed chains—where the first device is feeder-modem-connected and subsequent devices are daisy-chained on RS-485—are handled transparently.

## Bus Topology Architecture

```mermaid
flowchart LR 
    RU[Radio Unit<br/>AISG master] -->|"AISG bus (RS-485 or OOK on feeder)"| R1[RET #1<br/>Low-band array] 
    R1 --> R2[RET #2<br/>Mid-band array A] 
    R2 --> R3[RET #3<br/>Mid-band array B] 
    R3 --> TMA[TMA<br/>optional, same bus] 
    RU -.->|per-device MO| OM[O&M model<br/>RetDevice=1..3, TmaDevice=1]
```

## Operational Value and Limits

- **Operational Value:** Tilt optimization campaigns (manual or driven by SON/automated coverage optimization) can address every array on every site from the network management layer, with per-device calibration status, alarm supervision, and tilt read-back.
- **Capacity Limits:** A cascade of up to 12 ALDs per port is supported, with a total bus current budget enforced by the radio unit hardware.

# Cross-References

- [FEATURE DEPEDENCIES](feature-depedencies.md)
- [FEATURE OPERATION](feature-operation.md)
- [PARAMETERS](parameters.md)
- [ACTIVATION PROCEDURE](activation-procedure.md)
