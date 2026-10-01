---
type: concept
resource: data/vodafone-mvp/raw/NR Downlink Beam Optimizer.pdf#feature-depedencies
title: Feature Dependencies
description: Details the feature, hardware, and network dependencies, as well as limitations
  of the NR Downlink Beam Optimizer.
tags:
- dependencies
- limitations
- hardware-requirements
- licensing
- nr-downlink-beam-optimizer
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T17:02:19+00:00'
  source_sha256: '6907192936486100'
sources:
- resource: data/vodafone-mvp/raw/NR Downlink Beam Optimizer.pdf
  title: NR Downlink Beam Optimizer
---

This section details the feature, hardware, and network dependencies, along with the operational limitations of the NR Downlink Beam Optimizer. Because the optimizer manipulates the common-channel beam configuration, coordination and exclusion rules are critical to prevent conflicts with other features.

## Feature Dependencies

* **License and Activation:** Requires a valid license key (`FAK-30170`) installed and `FeatureCtrl=DlBeamOptimizer` set to `ACTIVATED` under `NrFunction=1`.
* **Baseline Feature:** Requires the Massive MIMO Mid-Band or Massive MIMO High-Band baseline feature active on the cell.
* **Mutual Exclusions:** Mutually exclusive with NR Flexible Cell Shaping Low/Mid-Band and NR Flexible Cell Shaping High-Band on the same cell, as both features manipulate the SSB grid.
* **Feature Coordination:** Coordinates with NR Massive MIMO Sleep Mode. Grid changes are suspended while the cell is in a sleep level, and the optimizer resumes after wake-up.
* **Measurement Enrichment:** Per-beam input measurements benefit from NR UE Coverage Measurements being active, which enriches the spatial map with UE-reported RSRP samples.

## Hardware Dependencies

* **Radio Hardware:** Supported on AAS radios with 32 or 64 transceiver branches, radio hardware generation R2 or later. Grid reconfiguration without cell restart requires R2.
* **Grid Catalog:** Radios expose a catalog of supported grids; the optimizer selects only from that catalog.

## Network Dependencies

* **Neighbor Relations:** A grid change alters the cell's effective coverage footprint at the margins. Automatic Neighbor Relation (ANR) will adapt neighbor lists automatically, but mobility-parameter audits are recommended after major grid changes.
* **Core and Transport:** No core or transport dependencies.

## Limitations

* **SSB Beam Layer Only:** The optimizer only affects the SSB beam layer. CSI-RS/traffic beamforming is adapted by the baseline beamforming function, not by this feature.
* **Execution Frequency:** Grid changes are applied at most once per `minChangeInterval` (default 24 h) and only inside the configured low-traffic window. This is designed as a slow outer loop.
* **Minimum Sample Count:** Cells with fewer than `minSampleCount` beam-level samples per evaluation period are not optimized due to insufficient statistics.
* **SSB Discontinuity:** A grid change causes a brief (< 1 s) SSB discontinuity. Idle UEs reacquire within one SSB periodicity, and connected UEs are protected by pre-change measurement reconfiguration, but ongoing handovers within that second may fail.

# Cross-References

* [Feature Overview](feature-overview.md) — For an overview of the NR Downlink Beam Optimizer.
* [Feature Operation](feature-operation.md) — For details on how the optimizer evaluates and applies grid changes.
* [Parameters](parameters.md) — For details on `minChangeInterval` and `minSampleCount`.
* [Activation Procedure](activation-procedure.md) — For instructions on activating the feature.
