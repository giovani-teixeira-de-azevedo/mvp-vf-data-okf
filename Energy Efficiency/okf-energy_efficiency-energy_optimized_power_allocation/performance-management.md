---
type: concept
resource: data/vodafone-mvp/raw/Energy-Optimized Power Allocation.pdf#performance-management
title: Performance Management
description: Details key performance indicators (KPIs) and counters used to measure
  energy savings and monitor link performance for Energy-Optimized Power Allocation.
tags:
- performance-management
- kpis
- counters
- energy-saving
- radio-energy
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-25T14:48:45+00:00'
  source_sha256: 29a1dfc8b56eb471
sources:
- resource: data/vodafone-mvp/raw/Energy-Optimized Power Allocation.pdf
  title: Energy-Optimized Power Allocation
---

Performance management evaluates how much transmit energy the feature removes and monitors whether link performance (downlink BLER, retransmissions, cell throughput) remains at baseline. Evaluation requires collecting a two-week pre-activation baseline over matching hours, followed by post-activation comparison. All counters accumulate per cell over a 15-minute Result Output Period (ROP).

## Key Performance Indicators (KPIs)

- **Power Reduction Ratio**: The primary efficiency KPI. Healthy cells with mixed traffic show 15–40% of PDSCH allocations power-reduced, with an average reduction of 2–4 dB.
- **Guard Rate**: The safety KPI. Values above 2% of reduced allocations indicate that the margin is too tight or CQI reporting is too sparse, requiring an increase to `powerMargin` or `minCqiForReduction`.
- **Energy Saving**: Ties the feature mechanism to measured radio energy. This KPI will understate the benefit on radios without Power Amplifier (PA) bias tracking.

| KPI | Formula | Description |
| --- | --- | --- |
| Power Reduction Ratio | `ctrReducedAllocs / ctrPdschAllocs × 100` | Share of PDSCH allocations transmitted at reduced power (%) |
| Average Reduction Depth | `ctrReductionDbSum / ctrReducedAllocs` | Mean applied reduction (dB) |
| Guard Rate | `ctrGuardEvents / ctrReducedAllocs × 100` | Reductions revoked by the NACK guard (%) |
| Energy Saving | `ctrTxEnergySaved / ctrRadioEnergyConsumed × 100` | Estimated transmit energy saved (%) |

## Performance Counters

- `ctrReductionDbSum` accumulates the applied dB across all reduced allocations solely to compute the average reduction depth.
- `ctrGuardEvents` tracks the cell's fast-fading environment and should remain low and stable. A step change without a configuration change often correlates with a new interference source.
- `ctrTxEnergySaved` is model-based using the PA power curve.
- `ctrRadioEnergyConsumed` is measured at the radio power feed and is the value to use for sustainability reporting.

| Counter | Description | Range | Datatype |
| --- | --- | --- | --- |
| `ctrPdschAllocs` | PDSCH allocations scheduled | 0–2³¹ | int64 |
| `ctrReducedAllocs` | Allocations transmitted at reduced power | 0–2³¹ | int64 |
| `ctrReductionDbSum` | Sum of applied reductions (dB) | 0–2³¹ | int64 |
| `ctrGuardEvents` | Reductions suspended by the NACK guard | 0–2³¹ | int64 |
| `ctrTxEnergySaved` | Estimated transmit energy saved (Wh) per ROP | 0–2³¹ | int64 |
| `ctrRadioEnergyConsumed` | Measured radio unit energy (Wh) per ROP | 0–2³¹ | int64 |

# Cross-References

- [Parameters](parameters.md) - Configuration parameters including `powerMargin` and `minCqiForReduction`.
