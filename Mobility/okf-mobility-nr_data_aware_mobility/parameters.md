---
type: reference-table
resource: data/vodafone-mvp/raw/NR Data-Aware Mobility.pdf#parameters
title: Parameters
description: Configuration parameters per NR cell for the NR Data-Aware Mobility feature.
tags:
- NR
- Data-Aware Mobility
- Parameters
- Configuration
- RSRP
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T10:02:23+00:00'
  source_sha256: c3f7a9f65cd77925
sources:
- resource: data/vodafone-mvp/raw/NR Data-Aware Mobility.pdf
  title: NR Data-Aware Mobility
---

This section defines the configuration parameters configured per NR cell for the NR Data-Aware Mobility feature. These parameters control the activation mode, activity thresholds, deferral offsets, traffic gap search windows, and safety thresholds.

### Safety Floor Validation

The safety floor parameter `criticalRsrpFloor` must be validated carefully per morphology. It must sit above the RSRP level where the cell's uplink becomes unreliable, because a deferred handover still requires a functioning uplink to execute successfully.

### Parameter List

| Parameter | Description | Values | Datatype | Default |
| :--- | :--- | :--- | :--- | :--- |
| `dataAwareMobMode` | Enables the function on the cell | `DISABLED`, `TIMING_ONLY`, `TIMING_AND_SCORING` | enum | `DISABLED` |
| `activityBufferThr` | RLC buffer occupancy classifying a UE as HIGH activity | 1–1000 (kB) | int32 | 50 |
| `activityRateThr` | Scheduled throughput classifying a UE as HIGH activity | 1–1000 (Mbps) | int32 | 20 |
| `deferOffset` | Additional A3/A5 offset for deferrable events | 0–6 (dB) | int32 | 2 |
| `deferTtt` | Additional time-to-trigger for deferrable events | 0–1280 (ms) | int32 | 256 |
| `gapSearchWindow` | Max time to wait for a traffic gap once due | 0–1000 (ms) | int32 | 500 |
| `bufferGapThr` | RLC buffer level counting as a traffic gap | 0–10 (kB) | int32 | 2 |
| `criticalRsrpFloor` | Serving RSRP forcing immediate execution | -140–-44 (dBm) | int32 | -116 |
| `speedDeferDisableThr` | Estimated UE speed disabling deferral | 30–500 (km/h) | int32 | 120 |

# Cross-References

* [Feature Operation](feature-operation.md)
* [Activation Procedure](activation-procedure.md)
