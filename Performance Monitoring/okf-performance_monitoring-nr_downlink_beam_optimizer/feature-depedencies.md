---
type: concept
resource: data/vodafone-mvp/raw/NR Downlink Beam Optimizer.pdf#feature-depedencies
title: Feature Dependencies
description: Describes the feature, hardware, and network dependencies, as well as
  limitations of the NR Downlink Beam Optimizer.
tags:
- nr
- downlink-beam-optimizer
- dependencies
- limitations
- demo-testing
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T14:35:38+00:00'
  source_sha256: '6907192936486100'
sources:
- title: NR Downlink Beam Optimizer
  resource: data/vodafone-mvp/raw/NR Downlink Beam Optimizer.pdf
---

This section outlines the feature, hardware, and network dependencies, as well as the operational limitations of the NR Downlink Beam Optimizer. It details the licensing, baseline feature requirements, mutual exclusions, coordination mechanisms, and hardware constraints necessary for successful deployment.

## Feature Dependencies

The NR Downlink Beam Optimizer manipulates the common-channel beam configuration, which several other features also depend on or manipulate. Consequently, exclusion and coordination rules are critical:

*   **Licensing and Activation:** Requires a valid license key (`FAK-30170`) installed and the parameter `FeatureCtrl=DlBeamOptimizer` set to `ACTIVATED` under `NrFunction=1`.
*   **Baseline Features:** Requires the Massive MIMO Mid-Band or Massive MIMO High-Band baseline feature active on the cell.
*   **Mutual Exclusions:** Mutually exclusive with NR Flexible Cell Shaping Low/Mid-Band and NR Flexible Cell Shaping High-Band on the same cell, as both features manipulate the SSB grid.
*   **Coordination with Sleep Mode:** Coordinates with NR Massive MIMO Sleep Mode. Grid changes are suspended while the cell is in a sleep level, and the optimizer resumes operation after wake-up.
*   **Measurement Enrichment:** Per-beam input measurements benefit from NR UE Coverage Measurements being active, which enriches the spatial map with UE-reported RSRP samples.

## Hardware Dependencies

*   **Radio Support:** Supported on Active Antenna System (AAS) radios with 32 or 64 transceiver branches, radio hardware generation R2 or later. Grid reconfiguration without cell restart requires R2 or later hardware.
*   **Grid Catalog:** Radios expose a catalog of supported grids; the optimizer selects only from that catalog.

## Network Dependencies

*   **Neighbor Relations:** A grid change alters the cell's effective coverage footprint at the margins. Automatic Neighbor Relation (ANR) will adapt neighbor lists automatically, but mobility-parameter audits are recommended after major grid changes.
*   **Core and Transport:** No core or transport network dependencies.

## Limitations

*   **SSB Beam Layer Only:** The optimizer only affects the SSB beam layer. CSI-RS/traffic beamforming is adapted by the baseline beamforming function, not by this feature.
*   **Slow Outer Loop:** Grid changes are applied at most once per `minChangeInterval` (default 24 h) and only inside the configured low-traffic window. This is a slow outer loop by design.
*   **Minimum Sample Count:** Cells with fewer than `minSampleCount` beam-level samples per evaluation period are not optimized due to insufficient statistics.
*   **SSB Discontinuity:** A grid change causes a brief (< 1 s) SSB discontinuity. Idle UEs reacquire within one SSB periodicity, and connected UEs are protected by pre-change measurement reconfiguration, but ongoing handovers within that second may fail.

# Cross-References

*   [Feature Overview](feature-overview.md) — Overview of the NR Downlink Beam Optimizer feature.
*   [Feature Operation](feature-operation.md) — Details on how the optimizer operates, including the slow outer loop and grid changes.
*   [Parameters](parameters.md) — Configuration parameters such as `FeatureCtrl`, `minChangeInterval`, and `minSampleCount`.
*   [Activation Procedure](activation-procedure.md) — Step-by-step instructions for activating the feature and installing the license key.
