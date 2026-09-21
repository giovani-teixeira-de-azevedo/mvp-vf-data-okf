---
type: concept
resource: data/vodafone-mvp/raw/Multicabinet Control.pdf#feature-depedencies
title: Feature Dependencies
description: Specifies the physical, hardware, network, software, and license dependencies
  as well as structural limitations of Multicabinet Control.
tags:
- multicabinet-control
- dependencies
- limitations
- scu
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:45:41+00:00'
  source_sha256: 36013f23fbbc00aa
sources:
- resource: data/vodafone-mvp/raw/Multicabinet Control.pdf
  title: Multicabinet Control
---

This section outlines the software, license, hardware, and network dependencies required to deploy Multicabinet Control, as well as the structural and operational limitations of the feature.

## Overview

Physical dependencies include supported Support-Control Unit (SCU) hardware in every cabinet and control connectivity between them. Site surveys of cabinet inventory should be conducted prior to deployment. Third-party enclosures that lack a compatible SCU cannot achieve full control and can only be integrated using dry-contact external alarms.

## Feature Dependencies

- **Licensing and Configuration**: Deployment requires a valid license key (`FAK-51060`) and the parameter `FeatureCtrl=MulticabinetControl` set to `ACTIVATED` under `EquipmentFunction=1`.
- **Energy Metering**: When both features are active, energy metering aggregates consumption across all controlled cabinets.
- **Power and Load-Shedding**: Battery and power-outage behavior interacts with the node shutdown priority configuration. The load-shedding order should be reviewed after adding cabinets.
- **Feature Compatibility**: There are no conflicts with radio or transport features.

## Hardware Dependencies

- **SCU Requirements**: Each secondary cabinet must contain a supported support-control unit (SCU generation S2 or later) or a supported integrated power system with SCU functionality.
- **Control Connectivity**: Communication requires an internal site Ethernet or a dedicated support control bus. The maximum cable run between cabinets is 100 meters.
- **Third-Party Cabinets**: Cabinets from third-party manufacturers without an SCU can only be represented via external alarm inputs (dry contacts) on the nearest SCU, with no active control.

## Network Dependencies

- **Site-Internal Traffic**: There are no network dependencies outside the site. All control traffic remains strictly on the site-internal bus or local area network (LAN).

## Limitations

- **Cabinet Limit**: A maximum of 8 cabinets (1 primary and 7 secondary cabinets) are supported per node.
- **Primary SCU Dependency**: The SCU of the primary cabinet acts as the coordination master. In the event of primary SCU failure, secondary cabinets degrade to autonomous local control, maintaining their last setpoints and local protection logic (safe, but unsupervised).
- **Firmware Upgrades**: Firmware upgrades of secondary SCUs are serialized and occur one cabinet at a time.
- **Generation Mixing**: Mixing very old and new SCU generations may limit the available climate features to the lowest common set.

# Cross-References

- [Feature Overview](feature-overview.md) — For a high-level overview of the Multicabinet Control feature.
- [Activation Procedure](activation-procedure.md) — For instructions on enabling the licensing and configuration parameters.
- [Parameters](parameters.md) — For details on `FeatureCtrl` and other configuration settings.
