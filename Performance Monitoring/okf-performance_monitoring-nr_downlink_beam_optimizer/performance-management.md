---
type: concept
resource: data/vodafone-mvp/raw/NR Downlink Beam Optimizer.pdf#performance-management
title: Performance Management
description: Performance management, KPIs, and counters for monitoring and supervising
  the NR Downlink Beam Optimizer.
tags:
- performance-management
- kpi
- counters
- beam-optimization
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T17:02:03+00:00'
  source_sha256: 8f7561ee7465f5dd
sources:
- resource: data/vodafone-mvp/raw/NR Downlink Beam Optimizer.pdf
  title: NR Downlink Beam Optimizer
---

This section describes the performance management (PM) framework for the NR Downlink Beam Optimizer. It details how the optimizer consumes PM statistics as input and how its performance, stability, and network impact are supervised.

## Monitoring Strategy

Performance management for this feature has a dual character: the optimizer both consumes PM (per-beam statistics as its input) and must itself be supervised to ensure its changes improve network performance and remain stable.

The monitoring strategy is based on a before/after comparison around each grid change:
* **Before the change:** Baseline traffic-weighted RSRP, accessibility, and beam-failure KPIs are evaluated over the evaluation period (`evalPeriod`) before the change.
* **After the change:** The same KPIs are compared over the guard period after the change.

Per-grid-version KPI aggregates are persisted so that long-term trending survives beam-identity changes. All counters are collected per cell over the standard 15-minute Result Output Period (ROP).

## Key Performance Indicators (KPIs)

* **Traffic-Weighted RSRP Gain:** Expected to be positive after each applied change. A near-zero long-run gain with frequent changes indicates that the `changeHysteresis` parameter is set too low.
* **Rollback Ratio:** Should be rare (well under 10% of changes). Repeated rollbacks on a single cell indicate that the spatial traffic pattern is bimodal across days (e.g., weekday vs. weekend), suggesting that the `evalPeriod` should be lengthened to a week.
* **Beam Failure Rate:** Should trend downward after optimization.

### KPI Formulas

| KPI | Formula | Description |
| :--- | :--- | :--- |
| **Traffic-Weighted RSRP Gain** | `(ctrTwRsrpSum / ctrTwRsrpSamples) − baseline` | Change in mean traffic-weighted SS-RSRP vs. pre-change baseline (dB) |
| **Grid Change Rate** | `ctrGridChanges / (days in period)` | Applied grid changes per day; should be $\ll 1$ |
| **Rollback Ratio** | `(ctrGridRollbacks / ctrGridChanges) × 100` | Share of applied changes auto-rolled back (%) |
| **Beam Failure Rate** | `(ctrBeamFailures / ctrBeamSwitchAttempts) × 100` | Beam failure events per beam-switch attempt (%) |

## Performance Counters

* `ctrTwRsrpSum` and `ctrTwRsrpSamples` implement the traffic-weighted RSRP measurement, where each UE RSRP sample is weighted by its concurrent traffic volume.
* `ctrGridRollbacks` serves as the primary safety indicator, tracking automatic rollbacks.
* `ctrEvalSkippedLowSamples` reveals cells that are silently not being optimized due to a lack of data (common on new sites, where the feature waits for sufficient traffic).

### Counter Definitions

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
| `ctrTwRsrpSum` | Sum of traffic-weighted SS-RSRP samples (dBm·samples) | $-2^{63}$ to $2^{63}$ | int64 |
| `ctrTwRsrpSamples` | Number of traffic-weighted RSRP samples | $0$ to $2^{63}$ | int64 |
| `ctrGridChanges` | Grid changes applied (closed loop or manual apply) | $0$ to $2^{31}$ | int64 |
| `ctrGridRollbacks` | Automatic rollbacks after failed guard period | $0$ to $2^{31}$ | int64 |
| `ctrBeamSwitchAttempts` | Intra-cell beam switch attempts | $0$ to $2^{63}$ | int64 |
| `ctrBeamFailures` | Beam failure recovery events | $0$ to $2^{31}$ | int64 |
| `ctrEvalSkippedLowSamples` | Evaluation cycles skipped for insufficient samples | $0$ to $2^{31}$ | int64 |
| `ctrGridVersion` | Grid version identifier active at ROP end | $0$ to $2^{31}$ | int32 |

# Cross-References

* [Feature Operation](feature-operation.md) — Details the closed-loop optimization process, evaluation cycles, and guard periods.
* [Parameters](parameters.md) — Defines configuration parameters such as `evalPeriod` and `changeHysteresis`.
