---
type: concept
resource: data/vodafone-mvp/raw/Mixed Bandwidth Support for Carrier Aggregation High-Band.pdf#feature-depedencies
title: Feature Dependencies
description: Describes the feature, hardware, and network dependencies, as well as
  operational limitations for Mixed Bandwidth Support for Carrier Aggregation High-Band.
tags:
- dependencies
- limitations
- carrier-aggregation
- fr2
- mixed-bandwidth
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:38:31+00:00'
  source_sha256: 01fd07f2c35f83ff
sources:
- title: Mixed Bandwidth Support for Carrier Aggregation High-
  resource: data/vodafone-mvp/raw/Mixed Bandwidth Support for Carrier Aggregation
    High-Band.pdf
---

This section details the feature, hardware, and network dependencies, as well as operational limitations for Mixed Bandwidth Support for Carrier Aggregation High-Band. Mixed bandwidth operation functions as an extension of the FR2 Carrier Aggregation (CA) baseline, establishing requirements for consistent bandwidth handling and supporting CA features.

## Feature Dependencies

- **FR2 CA Baseline Requirements:** Requires an active FR2 CA baseline configuration, specifically either:
  - NR 4CC DL Carrier Aggregation High-Band, or
  - NR 8CC DL Carrier Aggregation High-Band.
- **Licensing and Configuration:** Requires a valid license key (**FAK-33015**) and the parameter `FeatureCtrl=MixedBwCaHighBand` set to `ACTIVATED`.
- **Flexible Bandwidth Interworking:** Interworks with NR Flexible Channel Bandwidth for the definition of non-standard carrier sizes.
- **Uplink Compatibility:** Compatible with NR Uplink Carrier Aggregation High-Band; the same mixed bandwidth rules apply to uplink Component Carriers (CCs).
- **SCell Selection:** SCell selection among mixed-size carriers is improved when Dynamic Component Carrier Management is active, as carrier scores account for per-CC capacity.

## Hardware Dependencies

- **FR2 Radio Units:** The radio units must support the configured set of channel bandwidths. Currently, all AAS FR2 units support 50, 100, 200, and 400 MHz bandwidths.
- **Baseband Dimensioning:** Baseband capacity licensing counts the overall aggregated bandwidth rather than the raw CC count. Operators must verify that baseband dimensioning covers the enlarged aggregate.

## Network Dependencies

- **Core and Transport Network:** None. The feature functions strictly as a node-internal scheduling and configuration capability.
- **UE Capability:** Highly dependent on UE support. Only UEs signaling the relevant mixed band-combination entries can utilize the odd-sized CCs.

## Limitations

- **Subcarrier Spacing:** All CCs within a single CA configuration must use the same subcarrier spacing (120 kHz on FR2).
- **Contiguous Combinations:** Intra-band contiguous combinations featuring more than two distinct bandwidths in a single UE configuration are not supported in this release.
- **Fallback Group Restrictions:** A 400 MHz CC cannot be combined with 50 MHz CCs for UEs reporting fallback group restrictions.
- **Scheduling Restrictions:** Cross-carrier scheduling from a smaller-bandwidth CC toward a 400 MHz CC is not supported. Self-scheduling must be used on the 400 MHz CC.

# Cross-References

* [Feature Overview](feature-overview.md) — Technical context and overview of Mixed Bandwidth Support for Carrier Aggregation High-Band.
* [Activation Procedure](activation-procedure.md) — Details on activating the license key and the `FeatureCtrl` parameter.
* [Parameters](parameters.md) — Reference for configuration parameters including `FeatureCtrl`.
