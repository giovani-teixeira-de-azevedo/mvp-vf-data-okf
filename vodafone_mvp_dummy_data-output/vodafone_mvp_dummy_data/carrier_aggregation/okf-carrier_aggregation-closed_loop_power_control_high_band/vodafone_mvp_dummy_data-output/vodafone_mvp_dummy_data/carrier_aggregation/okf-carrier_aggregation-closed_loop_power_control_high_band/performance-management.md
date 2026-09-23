---
type: concept
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#performance-management
title: PERFORMANCE MANAGEMENT
description: Outlines performance management guidelines, Key Performance Indicators
  (KPIs), formulas, and counters for Closed-Loop Power Control High-Band.
tags:
- performance-management
- kpis
- counters
- closed-loop-power-control
- high-band
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T12:54:03+00:00'
  source_sha256: 3fc34b470724ed9e
sources:
- title: Closed-Loop Power Control High-Band
  resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
---

The Performance Management section for the Closed-Loop Power Control High-Band feature details the evaluation of loop convergence, interference reduction, and loop stability. Performance monitoring relies on collecting a two-week pre-activation baseline of uplink Interference over Thermal (IoT), uplink Block Error Rate (BLER), and cell-edge throughput over matching hours, followed by post-activation comparison. All counters are measured per cell per standard 15-minute Result Output Period (ROP).

## Key Performance Indicators (KPIs)

In a healthy cell, TPC Balance should settle between 40% and 60%. A strong bias toward up-commands indicates the target is set too high for the coverage design, while a bias toward down-commands suggests open-loop $p_0$ is too generous. An In-Window Ratio above 80% indicates good convergence; values below 60% warrant investigation into blockage frequency and beam switching rates. IoT Reduction is the headline benefit KPI and should trend 1.5–3 dB below baseline within a week.

| KPI | Formula | Description |
| --- | --- | --- |
| TPC Balance | `ctrTpcUpCmds / (ctrTpcUpCmds + ctrTpcDownCmds) × 100` | Share of TPC commands that are power-up (%) |
| In-Window Ratio | `ctrSinrInWindowSamples / ctrSinrSamples × 100` | Share of SINR samples inside the target window (%) |
| Loop Reset Rate | `ctrLoopResets / (ctrSinrSamples / 1000)` | Loop resets per 1000 SINR samples; high values indicate beam instability |
| Fast Ramp Rate | `ctrFastRampEvents / ctrTpcUpCmds × 100` | Share of up-corrections triggered by blockage recovery (%) |

## Performance Counters

The performance counters record loop activity and convergence quality:

- **`ctrLoopResets`**: Increments on every beam-switch-induced reset. A cell with a high value relative to its traffic is suffering beam churn, and power control tuning will not help until beam management is stabilized.
- **`ctrUeAtMaxPower`**: Identifies coverage-limited UEs that the loop cannot help. A rising trend suggests the cell is being asked to serve UEs beyond its FR2 link budget.

| Counter | Description | Range | Datatype |
| --- | --- | --- | --- |
| `ctrTpcUpCmds` | TPC power-up commands issued | 0–2³¹ | int64 |
| `ctrTpcDownCmds` | TPC power-down commands issued | 0–2³¹ | int64 |
| `ctrSinrSamples` | Filtered uplink SINR samples evaluated | 0–2³¹ | int64 |
| `ctrSinrInWindowSamples` | Samples inside the target window | 0–2³¹ | int64 |
| `ctrLoopResets` | Loop state resets (beam switch or RRC reconfiguration) | 0–2³¹ | int64 |
| `ctrFastRampEvents` | Blockage-recovery fast ramp activations | 0–2³¹ | int64 |
| `ctrUeAtMaxPower` | Samples where a UE reported PHR ≤ 0 at max power | 0–2³¹ | int64 |
