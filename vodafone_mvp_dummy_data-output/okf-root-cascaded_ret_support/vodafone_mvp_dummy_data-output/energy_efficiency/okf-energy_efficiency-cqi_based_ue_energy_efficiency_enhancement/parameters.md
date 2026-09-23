---
type: concept
resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf#parameters
title: PARAMETERS
description: Defines configuration parameters, default values, datatypes, and deployment
  guidelines for the CQI-Based UE Energy Efficiency Enhancement feature.
tags:
- parameters
- cqi
- energy-efficiency
- configuration
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T14:18:04+00:00'
  source_sha256: c49747ea04567636
sources:
- resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf
  title: CQI-Based UE Energy Efficiency Enhancement
---

This section specifies the configuration parameters for the CQI-Based UE Energy Efficiency Enhancement feature. Thresholds are expressed on the standard 0–15 CQI scale (4-bit wideband CQI, TS 38.214).

## Configuration Guidelines

* Defaults suit a typical mid-band macro cell. Indoor cells with compressed CQI distributions may lower `highCqiThr` by one step.
* Keep at least 3 CQI steps between `lowCqiThr` and `highCqiThr`.

## Parameter List

| Parameter | Description | Values | Datatype | Default |
| --- | --- | --- | --- | --- |
| `cqiEnergyEffMode` | Enables the function on the cell | `DISABLED`, `RACE_TO_SLEEP_ONLY`, `FULL` | enum | `DISABLED` |
| `highCqiThr` | Filtered CQI at or above which race-to-sleep applies | 0–15 | int32 | 11 |
| `lowCqiThr` | Filtered CQI at or below which relaxation applies | 0–15 | int32 | 5 |
| `cqiClassHysteresis` | Hysteresis on class transitions | 0–4 (CQI steps) | int32 | 1 |
| `cqiFilterTime` | Time constant of the CQI smoothing filter | 100–5000 (ms) | int32 | 1000 |
| `lowCqiHoldTimer` | Time low CQI must persist before relaxation | 1–60 (s) | int32 | 10 |
| `relaxedCsiPeriod` | Periodic CSI report interval for relaxed UEs | 20–320 (ms) | int32 | 80 |
| `mcsBackoff` | MCS index back-off for relaxed UEs | 0–4 | int32 | 2 |

# Cross-References

* [FEATURE OPERATION](feature-operation.md)
