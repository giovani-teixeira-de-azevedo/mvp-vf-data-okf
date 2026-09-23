---
type: concept
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#feature-depedencies
title: FEATURE DEPEDENCIES
description: Outlines feature, hardware, and network dependencies alongside system
  limitations for Cascaded RET Support.
tags:
- RET
- Cascaded RET
- Dependencies
- Hardware
- Limitations
- AISG
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T12:42:47+00:00'
  source_sha256: 241819c0b2ae119c
sources:
- title: Cascaded RET Support
  resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
---

The Cascaded RET Support feature builds on the basic antenna line device (ALD) management framework. Its dependencies are primarily at the physical layer, including bus power budgets, AISG protocol versions of installed actuators, and radio unit port capabilities. Reviewing site installation records prior to activation is recommended, as mis-declared cascades manifest as discovery failures rather than configuration errors.

## Feature Dependencies

* **Baseline Feature:** Requires the baseline RET Support feature to be activated. Cascaded RET Support extends this capability from single-device to multi-device operation per port.
* **Licensing and Control:** Requires a valid license key (`FAK-51020`) and parameter `FeatureCtrl=CascadedRetSupport` set to `ACTIVATED` under `EquipmentFunction=1`.
* **Interworking:** Interworks with Support for AISG ADB and TMA Support (FDD). Antenna Control Units / Antenna Device Beans (ADBs) and Tower Mounted Amplifiers (TMAs) may share the same cascade; the discovery scan enumerates them into their respective device MOs.
* **Higher-Layer Automation:** Tilt automation features (coverage optimization functions in the management system) rely on this feature to reach secondary actuators.

## Hardware Dependencies

* **Radio Units:** Radio units must provide an AISG-capable RET port or feeder-modem (bias-tee) capability. All macro radio hardware generations R1 and later qualify.
* **Actuators:** RET actuators must implement AISG v2.0 or v3.0. Legacy proprietary single-drop protocols are not scanned.
* **Power Budget:** The aggregate current draw of the cascade must stay within the port budget (typically 30 V DC, 2.0 A per port). Motor movement is serialized by the node to respect this budget.

## Network Dependencies

* **Scope:** None. The feature is node-local. The OSS detects and manages additional `RetDevice` MO instances through the standard configuration management interface.

## Limitations

* **Device Scale:** Maximum 12 ALDs per AISG port, and maximum 64 ALDs per node.
* **Movement Serialization:** Simultaneous tilt movement is limited to one actuator per bus (movements on different ports run in parallel).
* **Bus Mastership:** AISG v3.0 multi-primary configurations (where two nodes master the same bus) are not supported. Each bus must have exactly one master.
* **Firmware Downloads:** Device firmware download to cascaded ALDs is supported, but limited to one download per bus at a time.

# Cross-References

* [FEATURE OVERVIEW](feature-overview.md)
* [ACTIVATION PROCEDURE](activation-procedure.md)
* [PARAMETERS](parameters.md)
