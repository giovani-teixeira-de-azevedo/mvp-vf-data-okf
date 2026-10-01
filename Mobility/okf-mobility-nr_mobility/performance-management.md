---
type: concept
resource: data/vodafone-mvp/raw/NR Mobility.pdf#performance-management
title: Performance Management
description: Performance management guidelines, KPIs, and counters for monitoring
  NR Mobility health.
tags:
- NR Mobility
- Performance Management
- KPIs
- Counters
- MRO
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T09:18:55+00:00'
  source_sha256: 8dc17b1d2892d88e
sources:
- title: NR Mobility
  resource: data/vodafone-mvp/raw/NR Mobility.pdf
---

This section outlines the performance management framework for NR Mobility, detailing the key performance indicators (KPIs) and counters used to monitor handover success, Mobility Robustness Optimization (MRO) classifications, and user-plane interruption. These metrics form the permanent mobility health dashboard of the network.

## Monitoring Guidelines

To effectively monitor NR Mobility health, the following practices are recommended:
* **Baseline Comparison:** Compare each cell against its own historical performance and against its morphology-class peers.
* **Re-baselining:** After any threshold change, re-baseline performance over a period of at least one week.
* **Granularity:** All counters accumulate per cell (with per-relation breakdowns available) over a 15-minute Reporting Period (ROP).
* **Target Performance:** Healthy macro cells should exhibit:
  * Intra-frequency HO Execution Success $\ge$ 99.5%
  * Inter-frequency HO Execution Success $\ge$ 99%
  * Ping-Pong Ratio below 3%
  * Too-Late share dominating the failure classification.
* **Failure Analysis:**
  * A cell dominated by "too-early" failures is over-eager and requires more hysteresis or Time To Trigger (TTT).
  * A cell dominated by "too-late" failures with rising Radio Link Failure (RLF) requires the opposite adjustment or a per-relation offset.
  * Preparation failures point away from the radio toward Xn/NG transport or target-cell admission, and should be investigated as transport or capacity issues rather than tuning issues.

## Key Performance Indicators (KPIs)

The following KPIs are computed from the network counters:

| KPI | Formula | Description |
| :--- | :--- | :--- |
| **HO Preparation Success** | $\frac{\text{ctrHoPrepOk}}{\text{ctrHoPrepAttempt}} \times 100$ | Target-side preparation success (%) |
| **HO Execution Success** | $\frac{\text{ctrHoExecOk}}{\text{ctrHoExecAttempt}} \times 100$ | UE-arrival execution success (%) |
| **Ping-Pong Ratio** | $\frac{\text{ctrHoPingPong}}{\text{ctrHoExecOk}} \times 100$ | Handovers returning to source within 5 s (%) |
| **Too-Early Share** | $\frac{\text{ctrHoTooEarly}}{\text{ctrHoTooEarly} + \text{ctrHoTooLate} + \text{ctrHoWrongCell}} \times 100$ | Share of classified failures that were too early (%) |
| **Mean Interruption** | $\frac{\text{ctrHoInterruptSum}}{\text{ctrHoExecOk}}$ | Average user-plane interruption per handover (ms) |

## Counters

The MRO classification counters (`ctrHoTooEarly`, `ctrHoTooLate`, `ctrHoWrongCell`) are derived from UE re-establishment and RLF reports and serve as the ground truth for threshold tuning. These should be trended per relation, not only per cell, as a single bad border can be hidden inside a healthy cell aggregate.

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
| `ctrHoPrepAttempt` | Handover preparations sent (Xn + NG) | $0 - 2^{31}$ | int64 |
| `ctrHoPrepOk` | Preparations acknowledged by target | $0 - 2^{31}$ | int64 |
| `ctrHoExecAttempt` | Handover commands sent to UEs | $0 - 2^{31}$ | int64 |
| `ctrHoExecOk` | Handovers completed in target | $0 - 2^{31}$ | int64 |
| `ctrHoPingPong` | Completed handovers returning to source within 5 s | $0 - 2^{31}$ | int64 |
| `ctrHoTooEarly` | Failures classified as too-early | $0 - 2^{31}$ | int64 |
| `ctrHoTooLate` | Failures classified as too-late | $0 - 2^{31}$ | int64 |
| `ctrHoWrongCell` | Failures classified as wrong-cell | $0 - 2^{31}$ | int64 |
| `ctrHoInterruptSum` | Accumulated user-plane interruption (ms) | $0 - 2^{31}$ | int64 |

# Cross-References

* [Feature Operation](feature-operation.md) — Details on handover execution and MRO classification mechanisms.
* [Parameters](parameters.md) — Thresholds and timers (such as TTT and hysteresis) that influence these KPIs.
