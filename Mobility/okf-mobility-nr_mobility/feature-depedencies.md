---
type: concept
resource: data/vodafone-mvp/raw/NR Mobility.pdf#feature-depedencies
title: Feature Dependencies
description: Outlines the feature, hardware, and network dependencies, as well as
  the operational limitations for NR Mobility.
tags:
- NR Mobility
- Dependencies
- Hardware Dependencies
- Network Dependencies
- Limitations
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T09:18:59+00:00'
  source_sha256: 1af2fb0bb5b95d0c
sources:
- title: NR Mobility
  resource: data/vodafone-mvp/raw/NR Mobility.pdf
---

This section outlines the feature, hardware, and network dependencies, as well as the operational limitations for the NR Mobility feature. NR Mobility is positioned near the base of the feature stack, meaning it has few upward dependencies but serves as a foundation for many other features.

## Feature Dependencies

* **Intra-Frequency Mobility:** Included as part of the base NR software package.
* **Inter-Frequency Mobility:** Requires license key `FAK-33100` and the parameter `FeatureCtrl=NrMobility` set to `ACTIVATED`.
* **NR Automated Neighbor Relations (ANR):** Strongly recommended to populate and maintain the relation table.
* **Xn Configuration (NR):** Required for Xn-based handover. Without Xn configuration, all handovers default to the slower NG path.
* **Feature Extensions:** Extended by NR Data-Aware Mobility, User- and Service-Specific Mobility, and NR Traffic Offload, which modulate the triggers and decisions of this feature.
* **Measurement Gap-Aware NR Scheduling:** Recommended when inter-frequency measurements are enabled to limit the gap-related throughput cost.

## Hardware Dependencies

* None. Supported on all baseband unit generations.

## Network Dependencies

* **Core Network Interfaces:** NG interface to the Access and Mobility Management Function (AMF) is required (always present).
* **Xn Interfaces:** Required to neighbor gNodeBs for Xn-based handover.
* **Protocol Compatibility:** Neighbor gNodeBs must run compatible Xn Application Protocol (XNAP) versions per TS 38.423. Version negotiation handles up to one release of skew.
* **Inter-AMF Mobility:** The 5G Core (5GC) must support NG handover with AMF change.

## Limitations

* **Measurement Objects:** Maximum of 8 inter-frequency measurement objects per UE. Frequencies beyond this limit are prioritized by frequency-relation priority.
* **Conditional Handover (CHO):** CHO is not part of this feature.
* **Dual Connectivity:** Handover of UEs with active NR-NR Dual Connectivity follows NR-NR Dual Connectivity master-node (MN) procedures; this feature handles the MN leg only.
* **Cell Configuration Limits:** Per-UE measurement configuration is limited to a maximum of 64 simultaneously configured cells across all measurement objects.

# Cross-References

* [Feature Overview](feature-overview.md)
* [Feature Operation](feature-operation.md)
* [Network Impact](network-impact.md)
* [Parameters](parameters.md)
* [Performance Management](performance-management.md)
* [Activation Procedure](activation-procedure.md)
* [Deactivation Procedure](deactivation-procedure.md)
