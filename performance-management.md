---
type: concept
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#performance-management
title: Performance Management
description: Details performance management guidelines, baseline requirements, KPIs,
  and PM counters for Closed-Loop Power Control High-Band.
tags:
- power-control
- performance-management
- kpi
- counters
- fr2
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-24T14:32:47+00:00'
  source_sha256: 3fc34b470724ed9e
sources:
- title: Closed-Loop Power Control High-Band
  resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
---

Performance management for the Closed-Loop Power Control High-Band feature evaluates loop convergence, interference reduction, and loop stability. All counters are collected per cell per standard 15-minute Result Output Period (ROP), comparing post-activation performance against a two-week pre-activation baseline of uplink Interference over Thermal (IoT), uplink Block Error Rate (BLER), and cell-edge throughput over matching hours.

## Key Performance Indicators (KPIs)

In a healthy cell:
- **TPC Balance** should settle between 40% and 60%. A strong bias toward up-commands indicates the target is set too high for the coverage design, whereas a bias toward down-commands suggests open-loop $P_0$ is too generous.
- **In-Window Ratio** above 80% indicates good convergence. If it falls below 60%, blockage frequency and beam switching rates should be investigated.
- **IoT Reduction** is the headline benefit KPI and should trend 1.5–3 dB below baseline within a week.

| KPI | Formula | Description |
| --- | --- | --- |
| TPC Balance | `ctrTpcUpCmds / (ctrTpcUpCmds + ctrTpcDownCmds) × 100` | Share of TPC commands that are power-up (%) |
| In-Window Ratio | `ctrSinrInWindowSamples / ctrSinrSamples × 100` | Share of SINR samples inside the target window (%) |
| Loop Reset Rate | `ctrLoopResets / (ctrSinrSamples / 1000)` | Loop resets per 1000 SINR samples; high values indicate beam instability |
| Fast Ramp Rate | `ctrFastRampEvents / ctrTpcUpCmds × 100` | Share of up-corrections triggered by blockage recovery (%) |

## Performance Counters

Counters record loop activity and convergence quality:
- `ctrLoopResets`: Increments on every beam-switch-induced reset. A cell with a high value relative to its traffic suffers from beam churn; power control tuning will not help until beam management is stabilized.
- `ctrUeAtMaxPower`: Identifies coverage-limited UEs that the loop cannot help. A rising trend suggests the cell is serving UEs beyond its FR2 link budget.

| Counter | Description | Range | Datatype |
| --- | --- | --- | --- |
| `ctrTpcUpCmds` | TPC power-up commands issued | 0–2³¹ | int64 |
| `ctrTpcDownCmds` | TPC power-down commands issued | 0–2³¹ | int64 |
| `ctrSinrSamples` | Filtered uplink SINR samples evaluated | 0–2³¹ | int64 |
| `ctrSinrInWindowSamples` | Samples inside the target window | 0–2³¹ | int64 |
| `ctrLoopResets` | Loop state resets (beam switch or RRC reconfiguration) | 0–2³¹ | int64 |
| `ctrFastRampEvents` | Blockage-recovery fast ramp activations | 0–2³¹ | int64 |
| `ctrUeAtMaxPower` | Samples where a UE reported PHR ≤ 0 at max power | 0–2³¹ | int64 |
