---
type: concept
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#performance-management
title: PERFORMANCE MANAGEMENT
description: Performance management principles, KPIs, formulas, and counters for evaluating
  loop convergence and performance.
tags:
- performance-management
- kpis
- counters
- power-control
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T15:15:37+00:00'
  source_sha256: 3fc34b470724ed9e
sources:
- resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
  title: Closed-Loop Power Control High-Band
---

Performance management for this feature answers three questions: is the loop converging (UEs reaching the target window and staying there), is interference actually falling (IoT trend versus the pre-activation baseline), and is the loop stable (no oscillation between up and down commands). Collect a two-week pre-activation baseline of uplink IoT, uplink BLER, and cell-edge throughput over matching hours, then compare post-activation. All counters are per cell per standard 15-minute ROP.

## KPIs

In a healthy cell, TPC Balance should settle between 40% and 60% — a strong bias toward up-commands indicates the target is set too high for the coverage design, while a bias toward down-commands suggests open-loop p0 is too generous. In-Window Ratio above 80% indicates good convergence; below 60% investigate blockage frequency and beam switching rates. IoT Reduction is the headline benefit KPI and should trend 1.5–3 dB below baseline within a week.

| KPI | Formula | Description |
| --- | --- | --- |
| TPC Balance | `ctrTpcUpCmds / (ctrTpcUpCmds + ctrTpcDownCmds) × 100` | Share of TPC commands that are power-up (%) |
| In-Window Ratio | `ctrSinrInWindowSamples / ctrSinrSamples × 100` | Share of SINR samples inside the target window (%) |
| Loop Reset Rate | `ctrLoopResets / (ctrSinrSamples / 1000)` | Loop resets per 1000 SINR samples; high values indicate beam instability |
| Fast Ramp Rate | `ctrFastRampEvents / ctrTpcUpCmds × 100` | Share of up-corrections triggered by blockage recovery (%) |

## Counters

The counters record loop activity and convergence quality. `ctrLoopResets` deserves particular attention: it increments on every beam-switch-induced reset, so a cell with a high value relative to its traffic is suffering beam churn, and power control tuning will not help until beam management is stabilized. `ctrUeAtMaxPower` identifies coverage-limited UEs that the loop cannot help — a rising trend suggests the cell is being asked to serve UEs beyond its FR2 link budget.

| Counter | Description | Range | Datatype |
| --- | --- | --- | --- |
| `ctrTpcUpCmds` | TPC power-up commands issued | 0–2³¹ | int64 |
| `ctrTpcDownCmds` | TPC power-down commands issued | 0–2³¹ | int64 |
| `ctrSinrSamples` | Filtered uplink SINR samples evaluated | 0–2³¹ | int64 |
| `ctrSinrInWindowSamples` | Samples inside the target window | 0–2³¹ | int64 |
| `ctrLoopResets` | Loop state resets (beam switch or RRC reconfiguration) | 0–2³¹ | int64 |
| `ctrFastRampEvents` | Blockage-recovery fast ramp activations | 0–2³¹ | int64 |
| `ctrUeAtMaxPower` | Samples where a UE reported PHR ≤ 0 at max power | 0–2³¹ | int64 |
