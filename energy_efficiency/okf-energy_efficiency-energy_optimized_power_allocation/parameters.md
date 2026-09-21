---
type: reference-table
resource: data/vodafone-mvp/raw/Energy-Optimized Power Allocation.pdf#parameters
title: PARAMETERS
description: Configuration parameters and thresholds for controlling the Energy-Optimized
  Power Allocation feature.
tags:
- parameters
- power-allocation
- energy-optimization
- configuration
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:39:32+00:00'
  source_sha256: 14d2835267587f89
sources:
- resource: data/vodafone-mvp/raw/Energy-Optimized Power Allocation.pdf
  title: Energy-Optimized Power Allocation
---

This section defines the configuration parameters for the Energy-Optimized Power Allocation feature, providing the parameter names, descriptions, valid ranges, datatypes, and default values.

The `powerMargin` parameter acts as a safety knob, representing the SINR buffer retained above the Modulation and Coding Scheme (MCS) requirement before any power reduction is applied. The default value of 3 dB is designed to absorb CQI estimation errors and fast fading; it should only be reduced after confirming stable Block Error Rate (BLER) at the default setting. The `maxPowerReduction` parameter bounds the CQI-mismatch effect.

| Parameter | Description | Values | Datatype | Default |
| :--- | :--- | :--- | :--- | :--- |
| `energyPowerAllocMode` | Enables the function on the cell | DISABLED, ENABLED | enum | DISABLED |
| `powerMargin` | SINR headroom retained before reduction | 1–10 (dB) | int32 | 3 |
| `maxPowerReduction` | Maximum per-allocation PDSCH power reduction | 1–6 (dB) | int32 | 4 |
| `reductionStep` | Quantization step of the reduction | 1–3 (dB) | int32 | 1 |
| `guardTime` | Reduction suspension after consecutive NACKs | 10–1000 (ms) | int32 | 100 |
| `minCqiForReduction` | Minimum filtered CQI for a UE to be eligible | 0–15 | int32 | 9 |
| `paBiasTracking` | Enables PA bias adaptation on capable radios | OFF, ON | enum | ON |

# Cross-References

* [Feature Operation](feature-operation.md) — For details on how these parameters govern the power reduction and suspension behavior during operation.
* [Activation Procedure](activation-procedure.md) — For configuring these parameters during feature commissioning.
