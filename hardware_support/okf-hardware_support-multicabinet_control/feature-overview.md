---
type: concept
resource: data/vodafone-mvp/raw/Multicabinet Control.pdf#feature-overview
title: Feature Overview - Multicabinet Control
description: Multicabinet Control extends the node's equipment management domain across
  multiple physical cabinets, supervising secondary cabinet hardware from the primary
  baseband under a unified O&M model.
tags:
- multicabinet-control
- equipment-management
- baseband
- support-control-unit
- hardware-supervision
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:45:43+00:00'
  source_sha256: 0722bef01dd7ab28
sources:
- title: Multicabinet Control
  resource: data/vodafone-mvp/raw/Multicabinet Control.pdf
---

This section provides an overview of the Multicabinet Control feature, explaining how it extends equipment management across multiple physical enclosures at a single site. It outlines the architectural connection model, the unified managed object representation, and the operational benefits of centralized supervision.

## Introduction and Motivation

Macro cell sites often accumulate multiple physical cabinets over their lifecycle, such as an original LTE cabinet, an expansion cabinet for New Radio (NR) basebands, battery cabinets, and customer-equipment enclosures. 

Without Multicabinet Control, managing these additional cabinets presents a trade-off:
* Each additional cabinet requires its own support-control unit with separate O&M integration.
* Alternatively, auxiliary equipment runs unsupervised and remains invisible to fault management systems until a thermal shutdown occurs.

The **Multicabinet Control** feature addresses this by extending the primary baseband's equipment management domain to supervise and control support hardware in secondary cabinets. This includes:
* Power distribution and rectifiers
* Battery backup systems and temperature sensors
* Climate control systems (fans, heat exchangers, air conditioners)
* Security and environmental sensors (door and smoke sensors)

---

## Architectural and Operational Model

Multicabinet Control connects the Support Control Units (SCUs) of secondary cabinets to the support system of the primary cabinet. This connection is established over the internal site LAN or dedicated control cabling (such as a site control bus).

### Unified Management Tree
The entire physical assembly is modeled as a single equipment tree under the `EquipmentFunction=1` Managed Object (MO):
* Each individual enclosure is represented by one `Cabinet` MO.
* Climate, power, and sensor devices are represented as children of their respective `Cabinet` MO.

```mermaid
flowchart TB 
    subgraph C1[Primary cabinet] 
        BB[Baseband unit] --- SCU1[Support Control Unit 1] 
        SCU1 --- CL1[Climate: fans] 
        SCU1 --- PW1[Power: rectifiers] 
    end 
    subgraph C2[Expansion cabinet] 
        SCU2[Support Control Unit 2] --- CL2[Climate: heat exchanger] 
        SCU2 --- SEN2[Door / smoke sensors] 
    end 
    subgraph C3[Battery cabinet] 
        SCU3[Support Control Unit 3] --- BAT[Battery strings + temp sensors] 
    end 
    SCU1 ---|site control bus| SCU2 
    SCU1 ---|site control bus| SCU3 
    BB -->|single O&M model<br/>Cabinet=1..3| OSS[OSS FM/CM/PM]
```

### Centralized Management Functions
* **Fault Management**: Alarms originating from any cabinet are routed through the node's single fault-management pipeline, using cabinet-scoped source identification.
* **Configuration Management**: Climate setpoints and battery test schedules are configured globally per cabinet from a single centralized interface.

---

## Coordinated Behaviors and Technical Benefits

The unified O&M model enables coordinated site-wide behaviors:

* **Battery-Backed Shutdown Priorities**: During a mains power outage, shutdown priorities can span multiple cabinets. For example, the node can shed non-critical loads on an expansion cabinet first to preserve battery capacity for primary operations.
* **Coordinated Climate Control**: Centralized climate control prevents conflicting configurations between adjacent enclosures.
* **Energy Management**: Site-wide energy data is fed directly into the Energy Metering function.
* **Scalability**: The node supports up to **8 cabinets** per node.
* **Operational Savings**: Eliminating separate management integrations typical saves several hours of integration effort per expansion cabinet, while providing ongoing O&M simplification.

# Cross-References

* [Feature Dependencies](feature-depedencies.md)
* [Feature Operation](feature-operation.md)
* [Network Impact](network-impact.md)
* [Parameters](parameters.md)
* [Performance Management](performance-management.md)
* [Activation Procedure](activation-procedure.md)
* [Deactivation Procedure](deactivation-procedure.md)
