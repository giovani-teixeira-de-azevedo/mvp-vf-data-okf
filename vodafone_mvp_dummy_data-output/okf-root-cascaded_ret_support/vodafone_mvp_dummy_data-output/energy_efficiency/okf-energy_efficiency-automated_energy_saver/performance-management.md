---
type: concept
resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf#performance-management
title: Performance Management
description: Performance management framework, baseline requirements, KPIs, and PM
  counters for the Automated Energy Saver feature.
tags:
- performance-management
- kpis
- counters
- automated-energy-saver
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T14:00:39+00:00'
  source_sha256: f3517e2bc5eb6e89
sources:
- title: Automated Energy Saver
  resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf
---

Performance management for the Automated Energy Saver feature evaluates automation gain, prediction model accuracy, and reactive reverts. All counters are collected per node or per cell over a 15-minute Result Output Period (ROP).

## Baseline Requirements

Before feature activation, establish a two-week pre-activation baseline using:
- Subordinate features' sleep-time counters
- Node energy counters (via Energy Metering)

After activation, compare like-for-like hours against this baseline.

## Key Performance Indicators (KPIs)

| KPI | Formula | Description | Evaluation & Guidance |
| --- | --- | --- | --- |
| **Prediction Accuracy** | `(1 − ctrForecastMissIntervals / ctrForecastIntervals) × 100` | Share of ROPs where the load forecast held within margin (%) | Should sit above 90% within a week of activation on cells with regular traffic. Persistently lower values indicate irregular load (consider `cellOptOut` or `CONSERVATIVE` level). |
| **Reactive Revert Rate** | `ctrReactiveWakeups / ctrAutoSleepActions × 100` | Automated sleep actions later reverted by reactive wake-up (%) | Key safety KPI; counts intervals where a subordinate feature had to wake reactively despite the forecast. Values above 5% mean the margin policy is too aggressive for that cell. |
| **Automation Gain** | `ctrAutoEnergySaved / ctrNodeEnergyConsumed × 100` | Model-estimated saving attributable to automation (%) | Quantifies the added value of the orchestration and is the KPI to report toward energy programs. |
| **Automation Coverage** | `ctrCellsUnderAuto / ctrCellsTotal × 100` | Share of cells under automated control (%) | Tracks coverage across configured cells on the node. |

## Performance Management Counters

Counters are split into decision counters (actions taken, forecasts made) and outcome counters (energy, reverts).

- **`ctrReactiveWakeups`**: Expected to be near zero in steady state. A step increase pinpoints the cell and date where the traffic pattern changed, providing a useful early-warning signal apart from energy management.
- **`ctrAutoEnergySaved`**: Model-based calculation. Use Energy Metering counters for audited sustainability reporting.

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

- [Parameters](parameters.md) — Configurable parameters such as cell opt-out configurations.
