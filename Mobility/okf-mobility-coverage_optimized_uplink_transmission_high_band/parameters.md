---
type: reference-table
resource: data/vodafone-mvp/raw/Coverage-Optimized Uplink Transmission High-Band.pdf#parameters
title: Parameters
description: Cell-level configuration parameters for Coverage-Optimized Uplink Transmission
  High-Band, including SINR thresholds, repetition factors, and resource limits.
tags:
- Parameters
- Configuration
- Uplink Optimization
- SINR Thresholds
- NR Cell
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T11:14:35+00:00'
  source_sha256: 8c90c2f7c856cecb
sources:
- title: Coverage-Optimized Uplink Transmission High-Band
  resource: data/vodafone-mvp/raw/Coverage-Optimized Uplink Transmission High-Band.pdf
---

This section defines the cell-level configuration parameters for the Coverage-Optimized Uplink Transmission High-Band feature. These parameters are configured per NR cell and control the activation, entry/exit thresholds, repetition factors, and resource allocation limits for UEs operating in coverage-optimized mode.

## Parameter Tuning Guidance

The two SINR thresholds form the primary tuning surface:
* `covEnterThr` should sit 2–3 dB above the SINR at which PUSCH BLER exceeds 10% for the smallest allocation.
* `covExitThr` should sit at least 3 dB above `covEnterThr` to provide hysteresis.

## Parameter List

| Parameter | Description | Values | Datatype | Default |
| :--- | :--- | :--- | :--- | :--- |
| `covOptUlTxMode` | Enables the function on the cell | `DISABLED`, `ADAPTIVE`, `ALWAYS_EDGE` | enum | `DISABLED` |
| `covEnterThr` | Filtered UL SINR below which coverage mode is entered | -10 to 20 (dB) | int32 | 0 |
| `covExitThr` | Filtered UL SINR above which coverage mode is exited | -10 to 20 (dB) | int32 | 5 |
| `covExitTimer` | Time SINR must stay above exit threshold before exit | 100 to 5000 (ms) | int32 | 500 |
| `repEnterThr` | UL SINR below which PUSCH repetition is added | -10 to 10 (dB) | int32 | -3 |
| `maxPuschRep` | Maximum PUSCH repetition factor | 1, 2, 4, 8 | enum | 4 |
| `maxPrbCovMode` | Maximum UL PRBs allocated in coverage mode | 1 to 50 | int32 | 8 |
| `covModeUeShareMax` | Max share of UL slots for coverage-mode UEs | 5 to 100 (%) | int32 | 30 |
| `mcsCeilingCovMode` | Highest MCS index allowed in coverage mode | 0 to 15 | int32 | 6 |

# Cross-References

* [Feature Operation](feature-operation.md) — Details how these parameters govern the transition and behavior of UEs in coverage-optimized mode.
* [Activation Procedure](activation-procedure.md) — Explains how to configure these parameters during feature activation.
* [Performance Management](performance-management.md) — Describes the counters used to monitor the impact of these parameter settings.
