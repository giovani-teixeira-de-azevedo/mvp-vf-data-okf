---
type: reference-table
resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf#parameters
title: Parameters
description: Configuration parameters for the CQI-Based UE Energy Efficiency Enhancement
  feature.
tags:
- cqi
- ue-energy-efficiency
- ran-parameters
- configuration
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:38:54+00:00'
  source_sha256: c49747ea04567636
sources:
- resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf
  title: CQI-Based UE Energy Efﬁciency Enhancement
---

This section provides details on the configuration parameters used to control the CQI-Based UE Energy Efficiency Enhancement feature. These parameters define the thresholds, timers, and hysteresis values that dictate how the network transitions UEs into optimized states.

## Parameter Reference Table

| Parameter | Description | Values | Datatype | Default |
| :--- | :--- | :--- | :--- | :--- |
| **cqiEnergyEffMode** | Enables the function on the cell. | `DISABLED`, `RACE_TO_SLEEP_ONLY`, `FULL` | enum | `DISABLED` |
| **highCqiThr** | Filtered CQI at or above which race-to-sleep applies. | 0–15 | int32 | 11 |
| **lowCqiThr** | Filtered CQI at or below which relaxation applies. | 0–15 | int32 | 5 |
| **cqiClassHysteresis** | Hysteresis on class transitions. | 0–4 (CQI steps) | int32 | 1 |
| **cqiFilterTime** | Time constant of the CQI smoothing filter. | 100–5000 (ms) | int32 | 1000 |
| **lowCqiHoldTimer** | Time low CQI must persist before relaxation. | 1–60 (s) | int32 | 10 |
| **relaxedCsiPeriod** | Periodic CSI report interval for relaxed UEs. | 20–320 (ms) | int32 | 80 |
| **mcsBackoff** | MCS index back-off for relaxed UEs. | 0–4 | int32 | 2 |

## Threshold Tuning Guidelines

* **CQI Scale:** Thresholds are expressed on the standard 0–15 CQI scale (4-bit wideband CQI, TS 38.214).
* **Macro vs. Indoor Cells:** The defaults suit a typical mid-band macro cell. Indoor cells with compressed CQI distributions may lower `highCqiThr` by one step.
* **Separation:** Maintain at least 3 CQI steps of separation between `lowCqiThr` and `highCqiThr`.

# Cross-References

* [Feature Operation](feature-operation.md) — For details on how these thresholds and parameters are evaluated.
* [Activation Procedure](activation-procedure.md) — For instructions on configuring and enabling these parameters.
