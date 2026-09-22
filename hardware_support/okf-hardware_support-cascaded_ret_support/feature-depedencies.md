---
type: concept
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#feature-depedencies
title: Feature Dependencies
description: Outlines the feature, hardware, network dependencies, and system limitations
  of the Cascaded RET Support feature.
tags:
- RET
- Cascaded RET
- Dependencies
- Hardware Requirements
- Limitations
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:42:43+00:00'
  source_sha256: 241819c0b2ae119c
sources:
- resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
  title: Cascaded RET Support
---

This section outlines the feature, hardware, network dependencies, and system limitations for Cascaded Remote Electrical Tilt (RET) Support. The feature builds on the basic antenna line device (ALD) management framework, with dependencies primarily situated at the physical layer.

It is recommended to walk through the site installation records before activation, as a mis-declared cascade shows up as discovery failures rather than configuration errors.

## Feature Dependencies

* **Baseline RET Support:** Requires the baseline RET Support feature to be activated. Cascaded RET Support extends this baseline from single-device to multi-device operation per port.
* **Licensing and Configuration:** Requires a valid license key (`FAK-51020`) and the parameter `FeatureCtrl=CascadedRetSupport` set to `ACTIVATED` under `EquipmentFunction=1`.
* **Interworking:** Interworks with *Support for AISG ADB* and *TMA Support (FDD)*. Antenna Device Boards (ADBs) and Tower Mounted Amplifiers (TMAs) may share the same cascade, and the discovery scan enumerates them into their respective device Managed Objects (MOs).
* **Tilt Automation:** Tilt automation features (such as coverage optimization functions in the management system) rely on Cascaded RET Support to reach secondary actuators.

## Hardware Dependencies

* **Radio Units:** Radio units must provide an AISG-capable RET port or feeder-modem (bias-tee) capability. All currently shipping macro radio hardware generations (R1 and later) qualify.
* **RET Actuators:** RET actuators must implement AISG v2.0 or v3.0. Legacy proprietary single-drop protocols are not scanned.
* **Power Budget:** The aggregate current draw of the cascade must stay within the port budget (typically 30 V DC, 2.0 A per port). Motor movement is serialized by the node to respect this power budget.

## Network Dependencies

* **None:** The feature is node-local. The OSS sees additional `RetDevice` MO instances through the normal configuration management interface.

## Limitations

* **ALD Limits:** A maximum of 12 ALDs are supported per AISG port, and a maximum of 64 ALDs per node.
* **Simultaneous Movement:** Simultaneous tilt movement is limited to one actuator per bus. Note that movements on different ports run in parallel.
* **Multi-Primary Configurations:** AISG v3.0 multi-primary configurations (where two nodes master the same bus) are not supported. Each bus must have exactly one master.
* **Firmware Download:** Device firmware download to cascaded ALDs is supported, but only one download is allowed per bus at a time.

# Cross-References

* [Feature Overview](feature-overview.md) — Describes the overall capabilities of Cascaded RET Support.
* [Activation Procedure](activation-procedure.md) — Steps for activating Cascaded RET Support, which requires the license and configuration parameters listed here.
* [Parameters](parameters.md) — Defines the specific configuration parameters, including `FeatureCtrl` settings.
