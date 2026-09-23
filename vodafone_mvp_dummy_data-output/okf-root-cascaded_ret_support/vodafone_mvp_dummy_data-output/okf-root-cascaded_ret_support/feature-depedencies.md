---
type: concept
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#feature-depedencies
title: Feature Dependencies
description: Defines the feature, hardware, and network dependencies, as well as operational
  limits, for Cascaded RET Support.
tags:
- ret
- cascaded-ret
- dependencies
- hardware-requirements
- limitations
- aisg
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T07:59:48+00:00'
  source_sha256: 241819c0b2ae119c
sources:
- title: Cascaded RET Support
  resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
---

This section details the feature, hardware, and network dependencies, as well as operational limits, for the Cascaded RET Support feature.

The feature builds on the basic antenna line device management framework, with dependencies primarily at the physical layer: bus power budget, AISG protocol version of installed actuators, and port capabilities of connected radio units. Site installation records should be reviewed prior to activation, as mis-declared cascades appear as discovery failures rather than configuration errors.

## Feature Dependencies

- **Baseline Feature:** Requires the baseline RET Support feature to be activated; Cascaded RET Support extends operation from single-device to multi-device per port.
- **License and Activation:** Requires a valid license key (`FAK-51020`) and `FeatureCtrl=CascadedRetSupport` set to `ACTIVATED` under `EquipmentFunction=1`.
- **Interworking:** Interworks with Support for AISG ADB and TMA Support (FDD). ADBs and TMAs may share the same cascade, and the discovery scan enumerates them into their respective device Managed Objects (MOs).
- **Management Functions:** Tilt automation features (coverage optimization functions in the management system) rely on this feature to reach secondary actuators.

## Hardware Dependencies

- **Radio Units:** Radio units must provide an AISG-capable RET port or feeder-modem (bias-tee) capability. All currently shipping macro radio hardware generations (R1 and later) qualify.
- **Actuators:** RET actuators must implement AISG v2.0 or v3.0; legacy proprietary single-drop protocols are not scanned.
- **Power Budget:** Aggregate current draw of the cascade must stay within the port budget (typically 30 V DC, 2.0 A per port). Motor movement is serialized by the node to respect this budget.

## Network Dependencies

- **Node Locality:** None. The feature is node-local; the OSS sees additional `RetDevice` MO instances through the normal configuration management interface.

## Limitations

- **Capacity Limits:** Maximum 12 Antenna Line Devices (ALDs) per AISG port, and maximum 64 ALDs per node.
- **Tilt Serialization:** Simultaneous tilt movement is limited to one actuator per bus (movements on different ports run in parallel).
- **Bus Topology:** AISG v3.0 multi-primary configurations (two nodes mastering the same bus) are not supported; each bus has exactly one master.
- **Firmware Download:** Device firmware download to cascaded ALDs is supported, but only one download per bus at a time.

# Cross-References

- [FEATURE OVERVIEW](feature-overview.md)
- [ACTIVATION PROCEDURE](activation-procedure.md)
