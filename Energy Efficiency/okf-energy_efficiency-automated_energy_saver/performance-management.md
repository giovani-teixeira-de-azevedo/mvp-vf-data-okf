---
type: concept
resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf#performance-management
title: PERFORMANCE MANAGEMENT
description: Performance management baseline requirements, KPIs, formulas, and counter
  definitions for Automated Energy Saver.
tags:
- performance-management
- kpi
- counters
- energy-saver
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-29T16:08:31+00:00'
  source_sha256: f3517e2bc5eb6e89
sources:
- title: Automated Energy Saver
  resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf
---

This section outlines the performance management strategy, key performance indicators (KPIs), and measurement counters for the Automated Energy Saver feature.

## Overview and Measurement Baseline

Performance management for this feature addresses three primary questions:
1. How much energy saving the automation adds.
2. How accurate the prediction model is.
3. Whether automated actions ever had to be reverted reactively.

To measure performance, establish a two-week pre-activation baseline of the subordinate features' sleep-time counters and the node energy counters (via Energy Metering). After activation, compare like-for-like hours against this baseline. All counters are per node or per cell over the 15-minute ROP (Report Output Period).

## Key Performance Indicators (KPIs)

- **Prediction Accuracy**: Should sit above 90% within a week of activation on cells with regular traffic. Persistently lower values indicate irregular load (consider `cellOptOut` or `CONSERVATIVE` level; see [PARAMETERS](parameters.md)).
- **Reactive Revert Rate**: The key safety KPI that counts intervals where a subordinate feature had to wake reactively despite the forecast. Values above 5% mean the margin policy is too aggressive for that cell.
- **Automation Gain**: Quantifies the added value of the orchestration and is the KPI to report toward energy programs.
- **Automation Coverage**: Measures the share of cells under automated control.

| KPI | Formula | Description |
| --- | --- | --- |
| Prediction Accuracy | `(1 − ctrForecastMissIntervals / ctrForecastIntervals) × 100` | Share of ROPs where the load forecast held within margin (%) |
| Reactive Revert Rate | `ctrReactiveWakeups / ctrAutoSleepActions × 100` | Automated sleep actions later reverted by reactive wake-up (%) |
| Automation Gain | `ctrAutoEnergySaved / ctrNodeEnergyConsumed × 100` | Model-estimated saving attributable to automation (%) |
| Automation Coverage | `ctrCellsUnderAuto / ctrCellsTotal × 100` | Share of cells under automated control (%) |

## Performance Counters

The counters split into decision counters (actions taken, forecasts made) and outcome counters (energy, reverts):

- `ctrReactiveWakeups`: Expected to be near zero in steady state. A step increase pinpoints the cell and date where the traffic pattern changed, providing a useful early-warning signal apart from energy management.
- `ctrAutoEnergySaved`: Model-based counter estimating energy saved by automated actions. For audited sustainability reporting, use Energy Metering counters.

| Counter | Description | Range | Datatype |
| --- | --- | --- | --- |
| `ctrForecastIntervals` | ROPs for which a forecast was produced | 0–2³¹ | int64 |
| `ctrForecastMissIntervals` | ROPs where actual load exceeded forecast plus margin | 0–2³¹ | int64 |
| `ctrAutoSleepActions` | Sleep/deepening actions issued by the engine | 0–2³¹ | int64 |
| `ctrAutoWakeActions` | Pre-emptive wake actions issued by the engine | 0–2³¹ | int64 |
| `ctrReactiveWakeups` | Reactive wake-ups overriding an automated sleep | 0–2³¹ | int64 |
| `ctrAutoEnergySaved` | Estimated energy saved by automated actions (Wh) per ROP | 0–2³¹ | int64 |
| `ctrNodeEnergyConsumed` | Measured node energy (Wh) per ROP | 0–2³¹ | int64 |
| `ctrCellsUnderAuto` | Cells under automated control at ROP end | 0–24 | int32 |
| `ctrCellsTotal` | Cells configured on the node at ROP end | 0–24 | int32 |

# Cross-References

- [PARAMETERS](parameters.md)
