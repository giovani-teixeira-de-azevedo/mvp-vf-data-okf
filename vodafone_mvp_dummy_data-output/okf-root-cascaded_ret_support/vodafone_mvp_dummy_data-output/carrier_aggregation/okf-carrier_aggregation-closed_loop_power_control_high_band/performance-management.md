---
type: concept
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#performance-management
title: PERFORMANCE MANAGEMENT
description: Details performance management evaluation, KPIs, and counters for Closed-Loop
  Power Control High-Band.
tags:
- performance-management
- kpi
- counters
- power-control
- high-band
- fr2
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T14:51:50+00:00'
  source_sha256: 3fc34b470724ed9e
sources:
- resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
  title: Closed-Loop Power Control High-Band
---

This section details performance management for the Closed-Loop Power Control High-Band feature, including evaluation principles, Key Performance Indicators (KPIs), and performance counters.

## Baseline & Evaluation Overview

Performance management for this feature addresses three core evaluation questions:
1. **Loop Convergence**: Whether UEs reach the target window and remain there.
2. **Interference Reduction**: Whether interference is falling by comparing Interference over Thermal (IoT) trends against the pre-activation baseline.
3. **Loop Stability**: Whether the loop is stable without oscillation between power-up and power-down commands.

A two-week pre-activation baseline of uplink IoT, uplink BLER, and cell-edge throughput must be collected over matching hours for post-activation comparison. All counters are measured per cell per standard 15-minute Reporting Observation Period (ROP).

## Key Performance Indicators (KPIs)

### Performance Expectations
- **TPC Balance**: In a healthy cell, TPC Balance should settle between 40% and 60%. A strong bias toward up-commands indicates the target is set too high for the coverage design, while a bias toward down-commands suggests open-loop `p0` is too generous.
- **In-Window Ratio**: Values above 80% indicate good convergence. Values below 60% warrant investigation into blockage frequency and beam switching rates.
- **IoT Reduction**: Headline benefit KPI that should trend 1.5–3 dB below baseline within a week.

### KPI Formulas

| KPI | Formula | Description |
| --- | --- | --- |
| TPC Balance | `ctrTpcUpCmds / (ctrTpcUpCmds + ctrTpcDownCmds) × 100` | Share of TPC commands that are power-up (%) |
| In-Window Ratio | `ctrSinrInWindowSamples / ctrSinrSamples × 100` | Share of SINR samples inside the target window (%) |
| Loop Reset Rate | `ctrLoopResets / (ctrSinrSamples / 1000)` | Loop resets per 1000 SINR samples; high values indicate beam instability |
| Fast Ramp Rate | `ctrFastRampEvents / ctrTpcUpCmds × 100` | Share of up-corrections triggered by blockage recovery (%) |

## Counters

Counters record loop activity and convergence quality:
- `ctrLoopResets`: Increments on every beam-switch-induced reset. High values relative to traffic indicate beam churn, where power control tuning will not help until beam management is stabilized.
- `ctrUeAtMaxPower`: Identifies coverage-limited UEs that the loop cannot assist. A rising trend suggests the cell is serving UEs beyond its FR2 link budget.

### Counter Reference

| Counter | Description | Range | Datatype |
| --- | --- | --- | --- |
| ctrTpcUpCmds | TPC power-up commands issued | 0–2³¹ | int64 |
| ctrTpcDownCmds | TPC power-down commands issued | 0–2³¹ | int64 |
| ctrSinrSamples | Filtered uplink SINR samples evaluated | 0–2³¹ | int64 |
| ctrSinrInWindowSamples | Samples inside the target window | 0–2³¹ | int64 |
| ctrLoopResets | Loop state resets (beam switch or RRC reconfiguration) | 0–2³¹ | int64 |
| ctrFastRampEvents | Blockage-recovery fast ramp activations | 0–2³¹ | int64 |
| ctrUeAtMaxPower | Samples where a UE reported PHR ≤ 0 at max power | 0–2³¹ | int64 |

# Cross-References

- [Parameters](parameters.md)
- [Feature Operation](feature-operation.md)
- [Activation Procedure](activation-procedure.md)
