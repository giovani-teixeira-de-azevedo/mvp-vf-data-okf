---
type: reference-table
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#parameters
title: Parameters
description: Parameter configuration details for Closed-Loop Power Control High-Band
  on NR sector carriers.
tags:
- closed-loop-pc
- parameters
- power-control
- high-band
- nr-sector-carrier
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:37:06+00:00'
  source_sha256: c7e81fe336ed2a07
sources:
- resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
  title: Closed-Loop Power Control High-Band
---

This section details the configuration parameters for Closed-Loop Power Control High-Band. The parameters are configured per NR sector carrier.

The target SINR (`pcTargetSinr`) is the primary tuning knob: raising it improves per-UE Modulation and Coding Scheme (MCS) at the cost of higher interference. The default value is selected for dense urban FR2 grids. The hysteresis window and step configuration should only be changed after observing loop stability counters.

| Parameter | Description | Values | Datatype | Default |
| :--- | :--- | :--- | :--- | :--- |
| `closedLoopPcEnabled` | Enables the closed loop on the sector carrier | true, false | boolean | false |
| `pcTargetSinr` | Target uplink SINR for the control loop | -5–30 (dB) | int32 | 12 |
| `pcHysteresis` | Half-width of the no-action window around the target | 0–6 (dB) | int32 | 1 |
| `pcStepUp` | TPC step for below-target correction | 1, 3 (dB) | enum | 1 |
| `pcStepDown` | TPC step for above-target correction | 1 (dB) | enum | 1 |
| `blockageDetectThr` | SINR drop triggering fast ramp-up mode | 6–20 (dB) | int32 | 10 |
| `loopResetOnBeamSwitch` | Reset accumulated state at beam change | true, false | boolean | true |
| `srsWeight` | Weight of SRS vs PUSCH DMRS in the SINR filter | 0–100 (%) | int32 | 30 |

# Cross-References

* [Feature Overview](feature-overview.md)
* [Feature Operation](feature-operation.md)
* [Performance Management](performance-management.md)
