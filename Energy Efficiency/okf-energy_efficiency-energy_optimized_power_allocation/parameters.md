---
type: concept
resource: data/vodafone-mvp/raw/Energy-Optimized Power Allocation.pdf#parameters
title: Parameters
description: Configuration parameters for the Energy-Optimized Power Allocation feature,
  including operating modes, margin thresholds, guard times, and PA bias settings.
tags:
- parameters
- configuration
- power-allocation
- energy-saving
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-25T14:48:43+00:00'
  source_sha256: 14d2835267587f89
sources:
- title: Energy-Optimized Power Allocation
  resource: data/vodafone-mvp/raw/Energy-Optimized Power Allocation.pdf
---

This section details the configurable parameters for the Energy-Optimized Power Allocation feature.

The margin serves as the safety buffer retained above the MCS requirement before any power reduction is applied. The default setting of 3 dB absorbs CQI estimation error and fast fading, and should only be reduced after confirming stable BLER at the default value. The `maxPowerReduction` parameter bounds the CQI-mismatch effect described in Limitations.

| Parameter | Description | Values | Datatype | Default |
| --- | --- | --- | --- | --- |
| `energyPowerAllocMode` | Enables the function on the cell | DISABLED, ENABLED | enum | DISABLED |
| `powerMargin` | SINR headroom retained before reduction | 1–10 (dB) | int32 | 3 |
| `maxPowerReduction` | Maximum per-allocation PDSCH power reduction | 1–6 (dB) | int32 | 4 |
| `reductionStep` | Quantization step of the reduction | 1–3 (dB) | int32 | 1 |
| `guardTime` | Reduction suspension after consecutive NACKs | 10–1000 (ms) | int32 | 100 |
| `minCqiForReduction` | Minimum filtered CQI for a UE to be eligible | 0–15 | int32 | 9 |
| `paBiasTracking` | Enables PA bias adaptation on capable radios | OFF, ON | enum | ON |
