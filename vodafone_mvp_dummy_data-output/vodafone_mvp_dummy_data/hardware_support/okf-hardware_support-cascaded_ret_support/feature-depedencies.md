---
type: concept
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#feature-depedencies
title: Feature Dependencies
description: Details the feature, hardware, network dependencies, and operating limitations
  for Cascaded RET Support.
tags:
- RET
- Cascaded RET
- AISG
- Hardware Dependencies
- License
- Limitations
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T15:10:27+00:00'
  source_sha256: 241819c0b2ae119c
sources:
- resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
  title: Cascaded RET Support
---

The Cascaded RET Support feature builds on the basic antenna line device (ALD) management framework, with dependencies primarily residing at the physical layer, including bus power budget, AISG protocol version of installed actuators, and radio unit port capabilities. Site installation records should be verified before activation, as mis-declared cascades manifest as discovery failures rather than configuration errors.

## Feature Dependencies

* **Baseline RET Support:** Requires the baseline RET Support feature to be activated; Cascaded RET Support extends this capability from single-device to multi-device operation per port.
* **Licensing & Managed Objects:** Requires a valid license key (`FAK-51020`) and parameter `FeatureCtrl=CascadedRetSupport` set to `ACTIVATED` under `EquipmentFunction=1`.
* **Interworking:** Interworks with Support for AISG ADB (Antenna Device Building-block) and TMA Support (FDD). ADBs and TMAs may share the same cascade, and the discovery scan enumerates them into their respective device Managed Objects (MOs).
* **Tilt Automation:** Tilt automation features (coverage optimization functions in the management system) rely on this feature to reach secondary actuators.

## Hardware Dependencies

* **Radio Units:** Radio units must provide an AISG-capable RET port or feeder-modem (bias-tee) capability. All currently shipping macro radio hardware generations (R1 and later) qualify.
* **Actuator Protocol:** RET actuators must implement AISG v2.0 or v3.0. Legacy proprietary single-drop protocols are not scanned.
* **Power Budget:** The aggregate current draw of the cascade must stay within the port budget (typically 30 V DC, 2.0 A per port). Motor movement is serialized by the node to respect this power budget.

## Network Dependencies

* **None:** The feature is node-local. The OSS sees additional `RetDevice` MO instances through the normal configuration management interface.

## Limitations

* **Device Capacity:** Maximum 12 ALDs per AISG port, and maximum 64 ALDs per node.
* **Tilt Serialization:** Simultaneous tilt movement is limited to one actuator per bus (movements on different ports run in parallel).
* **Bus Master Topology:** AISG v3.0 multi-primary configurations (two nodes mastering the same bus) are not supported; each bus has exactly one master.
* **Firmware Download:** Device firmware download to cascaded ALDs is supported, but only one download per bus at a time.

# Cross-References

* [Feature Overview](feature-overview.md)
* [Parameters](parameters.md)
* [Activation Procedure](activation-procedure.md)
