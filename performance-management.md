---
type: concept
resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf#performance-management
title: Performance Management
description: Details performance management for Automated Energy Saver, including
  baseline requirements, Key Performance Indicators (KPIs), and measurement counters.
tags:
- performance-management
- kpis
- counters
- energy-saver
- automation
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-24T08:37:30+00:00'
  source_sha256: f3517e2bc5eb6e89
sources:
- resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf
  title: Automated Energy Saver
---

Performance management for the Automated Energy Saver feature evaluates automation savings, prediction model accuracy, and reactive reversion of automated actions. It defines pre-activation baseline requirements, Key Performance Indicators (KPIs), and measurement counters collected per node or per cell over a 15-minute Result Output Period (ROP).

## Baseline and Evaluation Guidance

Performance management addresses three primary operational questions:
- How much saving the automation adds
- How accurate the prediction model is
- Whether automated actions ever had to be reverted reactively

A two-week pre-activation baseline must be established using the subordinate features' sleep-time counters and node energy counters (via Energy Metering). After activation, performance is evaluated by comparing like-for-like hours. All counters are tracked per node or per cell over the 15-minute ROP.

## Key Performance Indicators (KPIs)

- **Prediction Accuracy**: Should sit above 90% within a week of activation on cells with regular traffic; persistently lower values indicate irregular load (consider `cellOptOut` or `CONSERVATIVE` level).
- **Reactive Revert Rate**: The key safety KPI counting intervals where a subordinate feature had to wake reactively despite the forecast; values above 5% mean the margin policy is too aggressive for that cell.
- **Automation Gain**: Quantifies the added value of the orchestration and is the KPI to report toward energy programs.
- **Automation Coverage**: Measures the proportion of cells under automated control.

| KPI | Formula | Description |
| --- | --- | --- |
| Prediction Accuracy | `(1 − ctrForecastMissIntervals / ctrForecastIntervals) × 100` | Share of ROPs where the load forecast held within margin (%) |
| Reactive Revert Rate | `ctrReactiveWakeups / ctrAutoSleepActions × 100` | Automated sleep actions later reverted by reactive wake-up (%) |
| Automation Gain | `ctrAutoEnergySaved / ctrNodeEnergyConsumed × 100` | Model-estimated saving attributable to automation (%) |
| Automation Coverage | `ctrCellsUnderAuto / ctrCellsTotal × 100` | Share of cells under automated control (%) |

## Counters

Counters are split into decision counters (actions taken, forecasts made) and outcome counters (energy, reverts).

- **`ctrReactiveWakeups`**: Expected to be near zero in steady state. A step increase pinpoints the cell and date where the traffic pattern changed, serving as a useful early-warning signal independent of energy management.
- **`ctrAutoEnergySaved`**: Model-based calculation. Use Energy Metering counters for audited sustainability reporting.

| Counter | Description | Range | Datatype |
| --- | --- | --- | --- |
| ctrForecastIntervals | ROPs for which a forecast was produced | 0–2³¹ | int64 |
| ctrForecastMissIntervals | ROPs where actual load exceeded forecast plus margin | 0–2³¹ | int64 |
| ctrAutoSleepActions | Sleep/deepening actions issued by the engine | 0–2³¹ | int64 |
| ctrAutoWakeActions | Pre-emptive wake actions issued by the engine | 0–2³¹ | int64 |
| ctrReactiveWakeups | Reactive wake-ups overriding an automated sleep | 0–2³¹ | int64 |
| ctrAutoEnergySaved | Estimated energy saved by automated actions (Wh) per ROP | 0–2³¹ | int64 |
| ctrNodeEnergyConsumed | Measured node energy (Wh) per ROP | 0–2³¹ | int64 |
| ctrCellsUnderAuto | Cells under automated control at ROP end | 0–24 | int32 |
| ctrCellsTotal | Cells configured on the node at ROP end | 0–24 | int32 |

# Cross-References

- [PARAMETERS](parameters.md)
