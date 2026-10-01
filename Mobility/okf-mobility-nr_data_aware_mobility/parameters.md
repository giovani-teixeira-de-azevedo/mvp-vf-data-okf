---
type: reference-table
resource: data/vodafone-mvp/raw/NR Data-Aware Mobility.pdf#parameters
title: Parameters
description: Parameter configurations and descriptions for NR Data-Aware Mobility
  per NR cell.
tags:
- NR
- Mobility
- Parameters
- Configuration
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T09:06:56+00:00'
  source_sha256: c3f7a9f65cd77925
sources:
- resource: data/vodafone-mvp/raw/NR Data-Aware Mobility.pdf
  title: NR Data-Aware Mobility
---

This section provides the parameter configurations for the NR Data-Aware Mobility feature. These parameters are configured per NR cell and control the behavior of traffic-aware and speed-aware handover deferrals.

The safety floor `criticalRsrpFloor` is the parameter that must be validated most carefully per morphology. It must sit above the RSRP where the cell's uplink becomes unreliable, since a deferred handover still requires a working uplink to execute.

## Parameter List

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

* [Feature Operation](feature-operation.md) — Details how these parameters are applied during handover deferral decisions.
* [Activation Procedure](activation-procedure.md) — Describes how to enable the feature and configure these parameters.
