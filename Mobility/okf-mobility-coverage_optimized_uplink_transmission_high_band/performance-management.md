---
type: concept
resource: data/vodafone-mvp/raw/Coverage-Optimized Uplink Transmission High-Band.pdf#performance-management
title: Performance Management
description: Performance management guidelines, KPIs, and counters for evaluating
  the Coverage-Optimized Uplink Transmission High-Band feature.
tags:
- performance-management
- KPIs
- counters
- telecom
- uplink-coverage
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T11:14:38+00:00'
  source_sha256: e87c591cc6814ee7
sources:
- resource: data/vodafone-mvp/raw/Coverage-Optimized Uplink Transmission High-Band.pdf
  title: Coverage-Optimized Uplink Transmission High-Band
---

This section outlines the performance management framework for the Coverage-Optimized Uplink Transmission High-Band feature. It details the methodology for evaluating feature performance, including baseline collection, key performance indicators (KPIs), and the underlying performance counters.

## Evaluation Methodology

Performance management for this feature is designed to answer three primary questions:
1. **Engagement Frequency:** How often is coverage mode engaged, and is the threshold tuning matched to the actual cell-edge population?
2. **Mobility Outcomes:** Is the feature improving mobility outcomes, specifically handover success and Radio Link Failure (RLF) trends at the cell border?
3. **Capacity Cost:** What is the capacity cost in terms of repetition resource consumption?

### Baseline Collection
To evaluate the feature's impact, collect a **two-week pre-activation baseline** of the following metrics on the target cells over the same hours of the day, and compare them with post-activation results:
* Handover success rate
* Uplink (UL) RLF rate
* UL Physical Resource Block (PRB) utilization

All counters are collected per cell over the standard **15-minute Result Output Period (ROP)**.

---

## Key Performance Indicators (KPIs)

The KPIs are computed from the performance counters to assess the health, effectiveness, and resource cost of the feature.

### KPI Target Guidelines
* **Coverage Mode Ratio:** In a healthy high-band macro cell, the expected ratio is **3–15%** of connected-UE time. 
  * Values **above 25%** suggest the cell is over-extended or that the `covEnterThr` parameter is set too high.
  * Values **near zero** on a border cell indicate that the threshold is too low to engage before RLF occurs.
* **Edge Report Success:** This should exceed **98%** after activation. If it does not improve compared to the baseline, verify that PUCCH repetition resources are actually being assigned by checking if `ctrPucchRepAssigned` is incrementing.
* **Repetition Cost:** If the repetition cost exceeds **~8%** of UL capacity, it is recommended to reduce the `maxPuschRep` or `covModeUeShareMax` parameters.

### KPI Formulas

| KPI | Formula | Description |
| :--- | :--- | :--- |
| **Coverage Mode Ratio** | $\frac{\text{ctrCovModeUeTime}}{\text{ctrConnectedUeTime}} \times 100$ | Share of connected-UE time spent in coverage mode (%) |
| **Edge Report Success** | $\frac{\text{ctrEdgeMeasReportOk}}{\text{ctrEdgeMeasReportOk} + \text{ctrEdgeMeasReportFail}} \times 100$ | Delivery success of measurement reports from coverage-mode UEs (%) |
| **Repetition Cost** | $\frac{\text{ctrPuschRepSlots}}{\text{ctrUlSlotsTotal}} \times 100$ | Share of UL slots consumed by PUSCH repetitions (%) |
| **Coverage Mode Stability** | $\frac{\text{ctrCovModeEntries}}{\text{ctrCovModeUeTime} / 3600}$ | Coverage mode entries per UE-hour in coverage mode; high values indicate threshold ping-pong |

---

## Performance Counters

The following counters feed the KPIs and provide diagnostic depth:
* **`ctrEdgeMeasReportFail`:** Deserves particular attention. It counts measurement reports from coverage-mode UEs that were never successfully decoded before the UE was lost (the exact failure this feature is designed to prevent). This counter should trend toward zero within days of activation.
* **`ctrCovModeEntries` vs. `ctrCovModeUeTime`:** This comparison reveals ping-pong behavior.
* **`ctrPuschRepSlots`:** This is the direct capacity-cost measure and should be trended against busy-hour UL PRB utilization.

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
| `ctrCovModeUeTime` | Accumulated UE time in coverage-optimized mode per ROP | 0–2³¹ s | int64 |
| `ctrConnectedUeTime` | Accumulated RRC-connected UE time per ROP | 0–2³¹ s | int64 |
| `ctrCovModeEntries` | Number of coverage-mode entry transitions | 0–2³¹ | int64 |
| `ctrEdgeMeasReportOk` | Measurement reports decoded from coverage-mode UEs | 0–2³¹ | int64 |
| `ctrEdgeMeasReportFail` | Measurement reports lost from coverage-mode UEs before UE loss | 0–2³¹ | int64 |
| `ctrPuschRepSlots` | UL slots used for PUSCH repetitions | 0–2³¹ | int64 |
| `ctrUlSlotsTotal` | Total UL slots available per ROP | 0–2³¹ | int64 |
| `ctrPucchRepAssigned` | PUCCH long-format repetition assignments made | 0–2³¹ | int64 |

# Cross-References

* [Parameters](parameters.md) — For details on parameters such as `covEnterThr`, `maxPuschRep`, and `covModeUeShareMax`.
* [Activation Procedure](activation-procedure.md) — For instructions on activating the feature and initiating post-activation monitoring.
