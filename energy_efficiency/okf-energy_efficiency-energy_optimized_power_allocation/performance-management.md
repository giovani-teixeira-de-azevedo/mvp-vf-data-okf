---
type: concept
resource: data/vodafone-mvp/raw/Energy-Optimized Power Allocation.pdf#performance-management
title: Performance Management
description: Explains the performance management strategy, key performance indicators
  (KPIs), and counters for tracking transmit energy reduction and link performance.
tags:
- performance-management
- kpis
- counters
- power-allocation
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:39:26+00:00'
  source_sha256: 29a1dfc8b56eb471
sources:
- resource: data/vodafone-mvp/raw/Energy-Optimized Power Allocation.pdf
  title: Energy-Optimized Power Allocation
---

The Performance Management section outlines the methodology for evaluating the performance and efficiency of the Energy-Optimized Power Allocation feature. It details baseline requirements, Key Performance Indicators (KPIs), and specific measurement counters.

## PM Strategy & Baseline Collection

Performance management for this feature is designed to address two main questions:
1. **How much transmit energy** the feature removes from the system.
2. **Whether link performance** (DL BLER, retransmissions, throughput) remains at baseline levels.

### Evaluation Methodology
* **Baseline Period:** Collect a two-week pre-activation baseline of downlink BLER, cell throughput, and radio energy counters over matching hours.
* **Post-Activation Evaluation:** Compare post-activation data against the matching hours of the baseline period.
* **Granularity:** All counters accumulate per cell over a 15-minute Reporting Period (ROP).

---

## Key Performance Indicators (KPIs)

The primary efficiency metric is the **Power Reduction Ratio**. In healthy cells with mixed traffic, this ratio typically ranges between 15% and 40% of PDSCH allocations, yielding an average power reduction of 2–4 dB.

The safety metric is the **Guard Rate**. Values exceeding 2% of reduced allocations indicate that the safety margin is too tight or CQI reporting is too sparse. Under these circumstances, operators should raise parameters like `powerMargin` or `minCqiForReduction`.

The **Energy Saving** KPI links the algorithm's decisions to physical radio energy consumption. Note that on radios lacking power amplifier (PA) bias tracking, this KPI will understate the actual savings.

### KPI Formulas

| KPI | Formula | Description |
| :--- | :--- | :--- |
| **Power Reduction Ratio** | `ctrReducedAllocs / ctrPdschAllocs × 100` | Share of PDSCH allocations transmitted at reduced power (%) |
| **Average Reduction Depth** | `ctrReductionDbSum / ctrReducedAllocs` | Mean applied reduction (dB) |
| **Guard Rate** | `ctrGuardEvents / ctrReducedAllocs × 100` | Reductions revoked by the NACK guard (%) |
| **Energy Saving** | `ctrTxEnergySaved / ctrRadioEnergyConsumed × 100` | Estimated transmit energy saved (%) |

---

## Performance Counters

The individual counters track key scheduler decisions, applied power reduction, and energy consumption metrics.

### Technical Guidance on Counters
* **`ctrReductionDbSum`**: Accumulates the sum of applied reductions in decibels (dB) across all power-reduced allocations. Its primary purpose is to compute the average reduction depth.
* **`ctrGuardEvents`**: Deserves close monitoring. It should track the cell's fast-fading environment and remain low and stable. A step-change increase in this counter without config updates often points to a new external interference source.
* **`ctrTxEnergySaved`**: A model-based counter calculated using the radio unit's PA power curve.
* **`ctrRadioEnergyConsumed`**: Measured directly at the radio unit's power feed. This is the correct metric to use for official sustainability and environmental reporting.

### Counter Reference

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
| `ctrPdschAllocs` | PDSCH allocations scheduled | 0–2³¹ | int64 |
| `ctrReducedAllocs` | Allocations transmitted at reduced power | 0–2³¹ | int64 |
| `ctrReductionDbSum` | Sum of applied reductions (dB) | 0–2³¹ | int64 |
| `ctrGuardEvents` | Reductions suspended by the NACK guard | 0–2³¹ | int64 |
| `ctrTxEnergySaved` | Estimated transmit energy saved (Wh) per ROP | 0–2³¹ | int64 |
| `ctrRadioEnergyConsumed` | Measured radio unit energy (Wh) per ROP | 0–2³¹ | int64 |

# Cross-References

* [Parameters](parameters.md) — For configuration details on `powerMargin` and `minCqiForReduction`.
