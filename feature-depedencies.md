---
type: concept
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#feature-depedencies
title: FEATURE DEPEDENCIES
description: Outlines the feature, hardware, and network dependencies, as well as
  limitations, for Cascaded RET Support.
tags:
- RET
- Cascaded RET Support
- AISG
- Dependencies
- Limitations
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T18:52:39+00:00'
  source_sha256: 241819c0b2ae119c
sources:
- title: Cascaded RET Support
  resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
---

This section outlines the feature, hardware, and network dependencies, as well as system limitations, for the Cascaded RET Support feature within the antenna line device management framework.

## Feature Dependencies

- **Baseline RET Support**: Requires the baseline RET Support feature to be activated; Cascaded RET Support extends it from single-device to multi-device operation per port.
- **License and Control Parameter**: Requires a valid license key (`FAK-51020`) and `FeatureCtrl=CascadedRetSupport` set to `ACTIVATED` under `EquipmentFunction=1`.
- **Interworking**: Interworks with Support for AISG ADB and TMA Support (FDD); ADBs and TMAs may share the same cascade, and the discovery scan enumerates them into their respective device MOs.
- **Tilt Automation**: Tilt automation features (coverage optimization functions in the management system) rely on this feature to reach secondary actuators.

## Hardware Dependencies

- **Radio Units**: Radio units must provide an AISG-capable RET port or feeder-modem (bias-tee) capability; all currently shipping macro radio hardware generations (R1 and later) qualify.
- **RET Actuators**: RET actuators must implement AISG v2.0 or v3.0; legacy proprietary single-drop protocols are not scanned.
- **Power Budget**: The aggregate current draw of the cascade must stay within the port budget (typically 30 V DC, 2.0 A per port); motor movement is serialized by the node to respect this budget.

## Network Dependencies

- **None**: The feature is node-local; the OSS sees additional `RetDevice` MO instances through the normal configuration management interface.

## Limitations

- Maximum 12 ALDs per AISG port, and maximum 64 ALDs per node.
- Simultaneous tilt movement is limited to one actuator per bus (movements on different ports run in parallel).
- AISG v3.0 multi-primary configurations (two nodes mastering the same bus) are not supported; each bus has exactly one master.
- Device firmware download to cascaded ALDs is supported, but only one download per bus at a time.

## Cross-References

- [Feature Overview](feature-overview.md)
- [Parameters](parameters.md)
- [Activation Procedure](activation-procedure.md)
