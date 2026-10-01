---
type: reference-table
resource: data/vodafone-mvp/raw/NR Mobility.pdf#parameters
title: PARAMETERS
description: Configuration parameters for NR cell mobility, including A3, A5, and
  measurement thresholds.
tags:
- NR
- Mobility
- Parameters
- Handover
- A3
- A5
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T09:19:34+00:00'
  source_sha256: c693aea563325a5e
sources:
- title: NR Mobility
  resource: data/vodafone-mvp/raw/NR Mobility.pdf
---

This section provides the configuration parameters for NR cell mobility, including event thresholds, hysteresis, timers, and filtering coefficients. These parameters are configured per NR cell, with the option to override them on a per-relation basis using the cell individual offset.

### Tuning Guidelines

*   **A3 Hysteresis and Time-to-Trigger (TTT):** These parameters should be tuned jointly. The product of `hysteresisA3` and `timeToTriggerA3` determines the balance between ping-pong handovers and too-late handovers.
*   **MRO Statistics:** Mobility Robustness Optimization (MRO) statistics, specifically `ctrHoTooEarly` and `ctrHoTooLate`, indicate which side of the optimum the cell is currently operating on.

### Parameter List

| Parameter | Description | Values | Datatype | Default |
| :--- | :--- | :--- | :--- | :--- |
| `hysteresisA3` | Hysteresis for event A3 | 0–15 (dB, 0.5 steps) | int32 | 2 |
| `timeToTriggerA3` | Time-to-trigger for event A3 | 0–5120 (ms, enum steps) | int32 | 320 |
| `a3Offset` | A3 offset (neighbor better than serving) | -15–15 (dB) | int32 | 3 |
| `a5Threshold1Rsrp` | A5 serving-cell threshold | -140–-44 (dBm) | int32 | -112 |
| `a5Threshold2Rsrp` | A5 neighbor-cell threshold | -140–-44 (dBm) | int32 | -106 |
| `a2SearchThr` | Serving RSRP starting inter-frequency measurements | -140–-44 (dBm) | int32 | -106 |
| `a1StopThr` | Serving RSRP stopping inter-frequency measurements | -140–-44 (dBm) | int32 | -100 |
| `filterCoeffRsrp` | L3 filter coefficient for RSRP | fc0–fc19 | enum | fc4 |
| `t304Timer` | Handover execution supervision timer | 50–2000 (ms) | int32 | 1000 |
| `cellIndividualOffset` | Per-relation offset applied to the neighbor | -24–24 (dB) | int32 | 0 |

# Cross-References

*   [Feature Operation](feature-operation.md) — For details on how these parameters are used in mobility events.
*   [Performance Management](performance-management.md) — For details on MRO statistics such as `ctrHoTooEarly` and `ctrHoTooLate`.
