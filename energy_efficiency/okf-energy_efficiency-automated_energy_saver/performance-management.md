---
type: concept
resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf#performance-management
title: Performance Management
description: Performance management framework, KPIs, and counters for monitoring the
  Automated Energy Saver feature.
tags:
- performance-management
- KPIs
- counters
- energy-saving
- metrics
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:38:12+00:00'
  source_sha256: f3517e2bc5eb6e89
sources:
- title: Automated Energy Saver
  resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf
---

This section details the performance management (PM) framework, Key Performance Indicators (KPIs), and counters used to monitor and evaluate the Automated Energy Saver feature. Performance monitoring focuses on three core areas: the energy savings added by automation, the accuracy of the prediction model, and the frequency of reactive sleep-state reversions.

## Baseline and Monitoring Methodology

To accurately quantify the impact of the Automated Energy Saver feature, operators must establish a **two-week pre-activation baseline** using:
* Subordinate features' sleep-time counters.
* Node energy counters (via Energy Metering).

After activation, compare like-for-like hours against this baseline. All performance counters are collected per node or per cell over a **15-minute Recording Observation Period (ROP)**.

---

## Key Performance Indicators (KPIs)

* **Prediction Accuracy**: This KPI measures how well the load forecast predicts actual traffic conditions. It should sit **above 90%** within a week of activation on cells with regular traffic. Persistently lower values indicate irregular load, in which case operators should consider setting the cell to `cellOptOut` or using the `CONSERVATIVE` policy level (see [Parameters](parameters.md)).
* **Reactive Revert Rate**: A critical safety KPI that tracks how often a subordinate feature had to reactively wake up despite the automated sleep forecast. Values **above 5%** indicate that the margin policy configured for that cell is too aggressive.
* **Automation Gain**: Quantifies the added value of the orchestration engine. This is the primary KPI reported toward corporate energy-saving and sustainability programs.
* **Automation Coverage**: Measures the proportion of active cells under automated orchestration control.

### KPI Formulas

| KPI | Formula | Description |
| :--- | :--- | :--- |
| **Prediction Accuracy** | $\left(1 - \frac{\text{ctrForecastMissIntervals}}{\text{ctrForecastIntervals}}\right) \times 100$ | Share of ROPs where the load forecast held within margin (%) |
| **Reactive Revert Rate** | $\frac{\text{ctrReactiveWakeups}}{\text{ctrAutoSleepActions}} \times 100$ | Automated sleep actions later reverted by reactive wake-up (%) |
| **Automation Gain** | $\frac{\text{ctrAutoEnergySaved}}{\text{ctrNodeEnergyConsumed}} \times 100$ | Model-estimated saving attributable to automation (%) |
| **Automation Coverage** | $\frac{\text{ctrCellsUnderAuto}}{\text{ctrCellsTotal}} \times 100$ | Share of cells under automated control (%) |

---

## Performance Counters

The performance counters are split into two logical categories:
1. **Decision Counters**: Actions taken and forecasts made (e.g., actions issued, intervals forecasted).
2. **Outcome Counters**: Physical or model-based outcomes (e.g., energy consumed, energy saved, reactive wakeups).

### Key Counter Behaviors

* **`ctrReactiveWakeups`**: In a steady, well-optimized state, this counter is expected to be near zero. A step increase in this counter pinpointing a specific cell and date indicates that the traffic pattern has changed, providing a useful early-warning signal for radio network operations quite apart from energy management.
* **`ctrAutoEnergySaved`**: This is a model-based estimation of energy savings. For audited sustainability and corporate reporting, actual measured **Energy Metering** counters (`ctrNodeEnergyConsumed`) should be used.

### Counter Specifications

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
| **`ctrForecastIntervals`** | ROPs for which a forecast was produced | 0–$2^{31}$ | `int64` |
| **`ctrForecastMissIntervals`** | ROPs where actual load exceeded forecast plus margin | 0–$2^{31}$ | `int64` |
| **`ctrAutoSleepActions`** | Sleep/deepening actions issued by the engine | 0–$2^{31}$ | `int64` |
| **`ctrAutoWakeActions`** | Pre-emptive wake actions issued by the engine | 0–$2^{31}$ | `int64` |
| **`ctrReactiveWakeups`** | Reactive wake-ups overriding an automated sleep | 0–$2^{31}$ | `int64` |
| **`ctrAutoEnergySaved`** | Estimated energy saved by automated actions (Wh) per ROP | 0–$2^{31}$ | `int64` |
| **`ctrNodeEnergyConsumed`** | Measured node energy (Wh) per ROP | 0–$2^{31}$ | `int64` |
| **`ctrCellsUnderAuto`** | Cells under automated control at ROP end | 0–24 | `int32` |
| ****`ctrCellsTotal`**** | Cells configured on the node at ROP end | 0–24 | `int32` |

# Cross-References

* [Feature Overview](feature-overview.md) — For understanding the overall architecture and subordinate features.
* [Feature Operation](feature-operation.md) — For details on automated actions, sleep states, and the forecasting engine.
* [Parameters](parameters.md) — For details on the margin policy, `cellOptOut`, and policy sensitivity levels (`CONSERVATIVE`).
* [Activation Procedure](activation-procedure.md) — For step-by-step instructions on activating the feature and beginning baseline comparison.
