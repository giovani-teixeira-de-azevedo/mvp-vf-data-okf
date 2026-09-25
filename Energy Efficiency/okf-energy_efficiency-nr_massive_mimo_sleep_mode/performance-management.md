---
type: concept
resource: data/vodafone-mvp/raw/NR Massive MIMO Sleep Mode.pdf#performance-management
title: PERFORMANCE MANAGEMENT
description: Defines performance KPIs, raw counters, and monitoring guidelines for
  assessing energy savings and operational stability.
tags:
- performance-management
- kpis
- counters
- energy-saving
- sleep-mode
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-25T16:15:05+00:00'
  source_sha256: 88c0b4bfaff9d93f
sources:
- resource: data/vodafone-mvp/raw/NR Massive MIMO Sleep Mode.pdf
  title: NR Massive MIMO Sleep Mode
---

Performance management for this feature answers two key questions: whether the feature is saving energy (sleep time and estimated savings), and whether it is doing so safely without instability or user-visible degradation.

## Monitoring Guidelines

KPIs should be monitored for at least two weeks after feature activation, comparing results against a pre-activation baseline of cell throughput, accessibility, and drop-rate KPIs taken over the same hours of the day. All counters are collected per cell over the standard 15-minute Result Output Period (ROP).

In a well-tuned cell:
- **Sleep Ratio**: Expected to be 20–40% (dominated by night hours). A Sleep Ratio near zero on a low-traffic cell usually indicates that `connUsersEnterThr` is set below the number of stationary IoT/FWA devices camping on the cell.
- **Deep Sleep Ratio**: Expected to grow as confidence in the thresholds increases.
- **Sleep Stability**: Expected to be well under 4 entries per sleep hour. A high Sleep Stability value indicates threshold ping-pong, which can be resolved by widening the gap between `prbLoadEnterThr` and `prbLoadExitThr` or extending `sleepEnterTimer`.

## Key Performance Indicators

The KPIs are derived from the raw counters collected per cell.

| KPI | Formula | Description |
| --- | --- | --- |
| Sleep Ratio | `ctrSleepDuration / ctrCellAvailableTime × 100` | Share of cell-available time spent in any sleep level (%) |
| Deep Sleep Ratio | `ctrDeepSleepDuration / ctrSleepDuration × 100` | Share of sleep time spent in Deep Sleep (%) |
| Estimated Energy Saving | `ctrEnergySavedEstimate / ctrRadioEnergyConsumed × 100` | Estimated radio energy saved relative to always-on operation (%) |
| Sleep Stability | `ctrSleepEntries / (ctrSleepDuration / 3600)` | Sleep entries per hour of sleep; high values indicate threshold ping-pong |

## Performance Counters

The counters below are the raw inputs to the KPIs:

- `ctrSleepDuration` and `ctrDeepSleepDuration` are time-accumulating counters capped at the ROP length.
- `ctrSleepAborts` deserves attention on its own: a non-zero steady rate is normal in cells serving emergency traffic, but a sudden increase can indicate an upstream MPS configuration change.
- `ctrEnergySavedEstimate` is model-based (computed from branch-off time and the radio's characterized power curve), whereas `ctrRadioEnergyConsumed` is measured directly at the radio power feed; use the measured counter for energy reporting toward sustainability programs.

| Counter | Description | Range | Datatype |
| --- | --- | --- | --- |
| `ctrSleepDuration` | Accumulated time in any sleep level per ROP | 0–900 s | int64 |
| `ctrDeepSleepDuration` | Accumulated time in Deep Sleep per ROP | 0–900 s | int64 |
| `ctrSleepEntries` | Number of sleep entry transitions | 0–2³¹ | int64 |
| `ctrSleepAborts` | Sleep entries aborted due to emergency/MPS traffic | 0–2³¹ | int64 |
| `ctrEnergySavedEstimate` | Estimated energy saved (Wh) per ROP | 0–2³¹ | int64 |
| `ctrRadioEnergyConsumed` | Measured radio unit energy (Wh) per ROP | 0–2³¹ | int64 |

# Cross-References

- [PARAMETERS](parameters.md) — Configuration parameters affecting sleep thresholds and stability (`prbLoadEnterThr`, `prbLoadExitThr`, `sleepEnterTimer`, `connUsersEnterThr`).
