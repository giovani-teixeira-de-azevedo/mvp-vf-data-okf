---
type: concept
resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf#performance-management
title: Performance Management
description: Performance management guidelines, KPIs, formulas, and counters for the
  Automated Energy Saver feature.
tags:
- performance-management
- kpi
- counters
- automated-energy-saver
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T18:18:02+00:00'
  source_sha256: f3517e2bc5eb6e89
sources:
- title: Automated Energy Saver
  resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf
---

Performance management for the Automated Energy Saver feature provides mechanisms to measure automation gain, evaluate prediction model accuracy, and detect reactive revert actions across 15-minute Result Output Periods (ROPs).

## Overview and Baseline Establishment

Performance evaluation requires establishing a two-week pre-activation baseline using subordinate features' sleep-time counters and node energy counters (via Energy Metering). After activation, like-for-like hours are compared. All counters are evaluated per node or per cell over a 15-minute ROP.

Performance management answers three primary questions:
1. How much energy saving the automation adds.
2. How accurate the prediction model is.
3. Whether automated actions ever had to be reverted reactively.

## Key Performance Indicators (KPIs)

* **Prediction Accuracy**: Measures the proportion of ROPs where load forecasts stayed within margin. Should sit above 90% within a week of activation on cells with regular traffic. Persistently lower values indicate irregular load, suggesting configuration adjustments (such as setting `cellOptOut` or using the `CONSERVATIVE` level).
* **Reactive Revert Rate**: Key safety KPI counting intervals where a subordinate feature was forced to wake reactively despite the forecast. Values above 5% indicate that the margin policy is too aggressive for that cell.
* **Automation Gain**: Quantifies the added value of orchestration and serves as the primary metric for energy program reporting.
* **Automation Coverage**: Measures the proportion of configured cells actively under automated control.

| KPI | Formula | Description |
| :--- | :--- | :--- |
| Prediction Accuracy | `(1 − ctrForecastMissIntervals / ctrForecastIntervals) × 100` | Share of ROPs where the load forecast held within margin (%) |
| Reactive Revert Rate | `ctrReactiveWakeups / ctrAutoSleepActions × 100` | Automated sleep actions later reverted by reactive wake-up (%) |
| Automation Gain | `ctrAutoEnergySaved / ctrNodeEnergyConsumed × 100` | Model-estimated saving attributable to automation (%) |
| Automation Coverage | `ctrCellsUnderAuto / ctrCellsTotal × 100` | Share of cells under automated control (%) |

## Performance Counters

Counters are divided into decision counters (actions taken, forecasts made) and outcome counters (energy saved, reverts).

* **`ctrReactiveWakeups`**: Expected to be near zero in steady state. A step increase pinpoints the cell and date where the traffic pattern changed, serving as an early-warning signal independent of energy management.
* **`ctrAutoEnergySaved`**: Model-based calculation. Audited sustainability reporting should rely on Energy Metering counters instead.

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
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

- [Parameters](parameters.md) — Configuration parameters including cell opt-out and automation levels mentioned in performance tuning guidelines.
