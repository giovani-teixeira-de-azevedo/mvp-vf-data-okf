---
type: concept
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#performance-management
title: Performance Management
description: Details performance monitoring strategy, baseline collection guidelines,
  KPIs, and counters for the Closed-Loop Power Control High-Band feature.
tags:
- performance-management
- kpis
- counters
- power-control
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:37:19+00:00'
  source_sha256: 3fc34b470724ed9e
sources:
- resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
  title: Closed-Loop Power Control High-Band
---

This section details the performance management strategy, key performance indicators (KPIs), and counters used to monitor, evaluate, and troubleshoot the Closed-Loop Power Control High-Band feature.

## Overview

Performance management for this feature answers three core questions:
1. **Is the loop converging?** (Are UEs reaching the target window and staying there?)
2. **Is interference actually falling?** (What is the uplink IoT trend compared to the pre-activation baseline?)
3. **Is the loop stable?** (Is there any oscillation between up and down commands?)

### Baseline Collection
Before activating the feature, collect a **two-week pre-activation baseline** of the following metrics over matching hours, then compare them post-activation:
- Uplink IoT
- Uplink BLER
- Cell-edge throughput

All counters are per cell per standard 15-minute Reporting Output Period (ROP).

## Key Performance Indicators (KPIs)

In a healthy cell, the KPIs should align with the following targets and guidelines:
- **TPC Balance:** Should settle between **40% and 60%**. A strong bias toward up-commands indicates the target is set too high for the coverage design, while a bias toward down-commands suggests the open-loop p0 is too generous.
- **In-Window Ratio:** An In-Window Ratio **above 80%** indicates good convergence. If it falls **below 60%**, investigate blockage frequency and beam switching rates.
- **IoT Reduction:** This is the headline benefit KPI and should trend **1.5–3 dB below the baseline** within one week.

| KPI | Formula | Description |
| :--- | :--- | :--- |
| **TPC Balance** | `ctrTpcUpCmds / (ctrTpcUpCmds + ctrTpcDownCmds) * 100` | Share of TPC commands that are power-up (%) |
| **In-Window Ratio** | `ctrSinrInWindowSamples / ctrSinrSamples * 100` | Share of SINR samples inside the target window (%) |
| **Loop Reset Rate** | `ctrLoopResets / (ctrSinrSamples / 1000)` | Loop resets per 1000 SINR samples; high values indicate beam instability |
| **Fast Ramp Rate** | `ctrFastRampEvents / ctrTpcUpCmds * 100` | Share of up-corrections triggered by blockage recovery (%) |

## Counters

The performance monitoring counters record loop activity and convergence quality:
- **ctrLoopResets:** This counter increments on every beam-switch-induced reset. A cell with a high value relative to its traffic is suffering from beam churn, and power control tuning will not help until beam management is stabilized.
- **ctrUeAtMaxPower:** This counter identifies coverage-limited UEs that the loop cannot help. A rising trend suggests that the cell is being asked to serve UEs beyond its FR2 link budget.

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
| **ctrTpcUpCmds** | TPC power-up commands issued | 0–2³¹ | int64 |
| **ctrTpcDownCmds** | TPC power-down commands issued | 0–2³¹ | int64 |
| **ctrSinrSamples** | Filtered uplink SINR samples evaluated | 0–2³¹ | int64 |
| **ctrSinrInWindowSamples** | Samples inside the target window | 0–2³¹ | int64 |
| **ctrLoopResets** | Loop state resets (beam switch or RRC reconfiguration) | 0–2³¹ | int64 |
| **ctrFastRampEvents** | Blockage-recovery fast ramp activations | 0–2³¹ | int64 |
| **ctrUeAtMaxPower** | Samples where a UE reported PHR ≤ 0 at max power | 0–2³¹ | int64 |

# Cross-References

For more details on how these performance metrics interact with feature settings and behaviors, refer to the following:
* [Feature Overview](feature-overview.md)
* [Feature Operation](feature-operation.md)
* [Parameters](parameters.md)
* [Network Impact](network-impact.md)
