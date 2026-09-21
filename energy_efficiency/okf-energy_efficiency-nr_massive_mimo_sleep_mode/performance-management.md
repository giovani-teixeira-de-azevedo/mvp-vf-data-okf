---
type: concept
resource: data/vodafone-mvp/raw/NR Massive MIMO Sleep Mode.pdf#performance-management
title: Performance Management
description: Performance management guidelines, KPIs, and counter definitions for
  monitoring NR Massive MIMO Sleep Mode.
tags:
- massive-mimo
- sleep-mode
- kpis
- counters
- energy-saving
- performance-management
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:42:09+00:00'
  source_sha256: 88c0b4bfaff9d93f
sources:
- resource: data/vodafone-mvp/raw/NR Massive MIMO Sleep Mode.pdf
  title: NR Massive MIMO Sleep Mode
---

This section outlines the performance management guidelines, Key Performance Indicators (KPIs), and performance counters for monitoring the NR Massive MIMO Sleep Mode feature.

Performance management for this feature answers two key questions:
1. Is the feature actually saving energy (measured via sleep time and estimated savings)?
2. Is it doing so safely (ensuring no system instability or user-visible performance degradation)?

## Monitoring Guidelines

To assess the impact of this feature, monitor the KPIs below for at least **two weeks** after activation. Compare these metrics against a pre-activation baseline of cell throughput, accessibility, and drop-rate KPIs taken over the same hours of the day. All performance counters are collected per cell over the standard 15-minute Result Output Period (ROP).

## Key Performance Indicators (KPIs)

The KPIs are derived from raw performance counters. In a well-tuned cell, typical expected performance values are:
- **Sleep Ratio**: 20% to 40% (dominated by night hours).
- **Deep Sleep Ratio**: Should grow as confidence in the activation thresholds increases.
- **Sleep Stability**: Well under 4 entries per sleep hour.

### Troubleshooting Thresholds
- **High Sleep Stability**: Indicates threshold ping-pong. To resolve, widen the gap between the threshold parameters `prbLoadEnterThr` and `prbLoadExitThr`, or extend the `sleepEnterTimer`. Refer to [Parameters](parameters.md) for more details.
- **Sleep Ratio near zero in low-traffic cells**: This usually indicates that the connection threshold `connUsersEnterThr` is set below the number of stationary IoT/FWA devices camping on the cell.

### KPI Definitions

| KPI | Formula | Description |
|---|---|---|
| **Sleep Ratio** | `(ctrSleepDuration / ctrCellAvailableTime) * 100` | Share of cell-available time spent in any sleep level (%) |
| **Deep Sleep Ratio** | `(ctrDeepSleepDuration / ctrSleepDuration) * 100` | Share of sleep time spent in Deep Sleep (%) |
| **Estimated Energy Saving** | `(ctrEnergySavedEstimate / ctrRadioEnergyConsumed) * 100` | Estimated radio energy saved relative to always-on operation (%) |
| **Sleep Stability** | `ctrSleepEntries / (ctrSleepDuration / 3600)` | Sleep entries per hour of sleep; high values indicate threshold ping-pong |

---

## Performance Counters

These raw counters are the inputs to the KPIs. 
- `ctrSleepDuration` and `ctrDeepSleepDuration` are time-accumulating counters capped at the 15-minute ROP length (900 seconds).
- `ctrSleepAborts` deserves specific attention: a non-zero steady rate is normal in cells serving emergency traffic. However, a sudden increase can indicate an upstream Multimedia Priority Service (MPS) configuration change.
- `ctrEnergySavedEstimate` is model-based, computed from the branch-off time and the radio unit's characterized power curve.
- `ctrRadioEnergyConsumed` is measured directly at the radio power feed. This measured counter should be preferred for energy reporting toward corporate sustainability programs.

### Counter Definitions

| Counter | Description | Range | Datatype |
|---|---|---|---|
| `ctrSleepDuration` | Accumulated time in any sleep level per ROP | 0–900 s | int64 |
| `ctrDeepSleepDuration` | Accumulated time in Deep Sleep per ROP | 0–900 s | int64 |
| `ctrSleepEntries` | Number of sleep entry transitions | 0–2³¹ | int64 |
| `ctrSleepAborts` | Sleep entries aborted due to emergency/MPS traffic | 0–2³¹ | int64 |
| `ctrEnergySavedEstimate` | Estimated energy saved (Wh) per ROP | 0–2³¹ | int64 |
| `ctrRadioEnergyConsumed` | Measured radio unit energy (Wh) per ROP | 0–2³¹ | int64 |

# Cross-References

* [Parameters](parameters.md) — Configuration thresholds (`prbLoadEnterThr`, `prbLoadExitThr`, `sleepEnterTimer`, `connUsersEnterThr`) referenced in KPI tuning and troubleshooting.
