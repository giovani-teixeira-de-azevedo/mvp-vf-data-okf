---
type: concept
resource: data/vodafone-mvp/raw/NR Downlink Beam Optimizer.pdf#performance-management
title: Performance Management
description: Performance management, KPIs, and counters for the NR Downlink Beam Optimizer
  feature.
tags:
- performance-management
- KPIs
- counters
- NR
- beam-optimization
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T14:20:58+00:00'
  source_sha256: 8f7561ee7465f5dd
sources:
- title: NR Downlink Beam Optimizer
  resource: data/vodafone-mvp/raw/NR Downlink Beam Optimizer.pdf
---

The performance management (PM) framework for the NR Downlink Beam Optimizer feature serves a dual purpose: it provides the per-beam statistics consumed by the optimizer as input, and it enables supervision of the optimizer's stability and effectiveness.

## Monitoring Strategy

The core monitoring strategy relies on a before-and-after comparison around each grid change:
* **Baseline Period:** Traffic-weighted RSRP, accessibility, and beam-failure KPIs are evaluated over the `evalPeriod` before a change.
* **Guard Period:** The same KPIs are compared over the guard period following the change.

To ensure long-term trending survives beam-identity changes, per-grid-version KPI aggregates are persisted. All counters are collected per cell over the standard 15-minute Result Output Period (ROP).

## Key Performance Indicators (KPIs)

The following KPIs are used to monitor the performance and stability of the optimizer:

* **Traffic-Weighted RSRP Gain:** Expected to be positive after each applied change. A near-zero long-run gain combined with frequent changes indicates that the `changeHysteresis` parameter is set too low.
* **Rollback Ratio:** Should be rare (well under 10% of changes). Repeated rollbacks on a single cell indicate that the spatial traffic pattern is bimodal across days (e.g., weekday vs. weekend), suggesting that the `evalPeriod` should be lengthened to a week.
* **Beam Failure Rate:** Should trend downward after optimization.

| KPI | Formula | Description |
| :--- | :--- | :--- |
| **Traffic-Weighted RSRP Gain** | `(ctrTwRsrpSum / ctrTwRsrpSamples) − baseline` | Change in mean traffic-weighted SS-RSRP vs. pre-change baseline (dB) |
| **Grid Change Rate** | `ctrGridChanges / (days in period)` | Applied grid changes per day; should be $\ll 1$ |
| **Rollback Ratio** | `(ctrGridRollbacks / ctrGridChanges) × 100` | Share of applied changes auto-rolled back (%) |
| **Beam Failure Rate** | `(ctrBeamFailures / ctrBeamSwitchAttempts) × 100` | Beam failure events per beam-switch attempt (%) |

## Performance Counters

The feature collects several counters per cell to support KPI calculation and operational visibility. 

* `ctrTwRsrpSum` and `ctrTwRsrpSamples` implement the traffic-weighted RSRP measurement, where each UE RSRP sample is weighted by its concurrent traffic volume.
* `ctrGridRollbacks` serves as the primary safety indicator.
* `ctrEvalSkippedLowSamples` identifies cells that are not being optimized due to insufficient data (common on new sites where the feature waits for traffic).

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
| `ctrTwRsrpSum` | Sum of traffic-weighted SS-RSRP samples (dBm·samples) | $-2^{63}$ to $2^{63}$ | `int64` |
| `ctrTwRsrpSamples` | Number of traffic-weighted RSRP samples | $0$ to $2^{63}$ | `int64` |
| `ctrGridChanges` | Grid changes applied (closed loop or manual apply) | $0$ to $2^{31}$ | `int64` |
| `ctrGridRollbacks` | Automatic rollbacks after failed guard period | $0$ to $2^{31}$ | `int64` |
| `ctrBeamSwitchAttempts` | Intra-cell beam switch attempts | $0$ to $2^{63}$ | `int64` |
| `ctrBeamFailures` | Beam failure recovery events | $0$ to $2^{31}$ | `int64` |
| `ctrEvalSkippedLowSamples` | Evaluation cycles skipped for insufficient samples | $0$ to $2^{31}$ | `int64` |
| `ctrGridVersion` | Grid version identifier active at ROP end | $0$ to $2^{31}$ | `int32` |

# Cross-References

* [Parameters](parameters.md) — For details on `evalPeriod` and `changeHysteresis`.
* [Feature Operation](feature-operation.md) — For details on grid changes, guard periods, and rollback mechanisms.
