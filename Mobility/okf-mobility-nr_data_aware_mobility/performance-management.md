---
type: concept
resource: data/vodafone-mvp/raw/NR Data-Aware Mobility.pdf#performance-management
title: Performance Management
description: Performance management guidelines, KPIs, and counters for monitoring
  NR Data-Aware Mobility.
tags:
- NR Data-Aware Mobility
- Performance Management
- KPIs
- Counters
- RAN
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T10:02:24+00:00'
  source_sha256: 3cbc917c8ae5ccd4
sources:
- resource: data/vodafone-mvp/raw/NR Data-Aware Mobility.pdf
  title: NR Data-Aware Mobility
---

This section outlines the performance management framework for the NR Data-Aware Mobility feature. It defines the key performance indicators (KPIs) and performance counters used to monitor, evaluate, and tune the feature's behavior, ensuring that traffic-gap handovers are executed with minimal risk and maximum throughput benefit.

## Performance Monitoring and Baselining

To evaluate the performance of the NR Data-Aware Mobility feature, operators should establish a baseline of the cell's performance before activation and compare it over a period of at least two weeks. The baseline should include:
* Handover success rate
* Ping-pong rate
* Per-UE throughput-during-mobility

All performance counters accumulate per cell over a 15-minute Recording Observation Period (ROP).

## Key Performance Indicators (KPIs)

The following KPIs are calculated using the performance counters to assess the health and efficiency of the configuration. A healthy configuration typically exhibits a **Gap Execution Ratio of 40–70%** and an **Escalation Ratio below 5%**.

| KPI | Formula | Description |
| :--- | :--- | :--- |
| **Deferral Ratio** | $\frac{\text{ctrHoDeferred}}{\text{ctrHoTriggeredDeferable}} \times 100$ | Share of deferrable handovers actually deferred (%) |
| **Gap Execution Ratio** | $\frac{\text{ctrHoGapExecuted}}{\text{ctrHoDeferred}} \times 100$ | Deferred handovers executed inside a traffic gap (%) |
| **Escalation Ratio** | $\frac{\text{ctrHoEscalated}}{\text{ctrHoDeferred}} \times 100$ | Deferred handovers escalated by the RSRP floor (%) |
| **Deferred HO Failure** | $\frac{\text{ctrHoDeferredFail}}{\text{ctrHoDeferred}} \times 100$ | Failure rate of deferred handovers (%) |
| **Scoring Redirect Share** | $\frac{\text{ctrHoScoredRedirect}}{\text{ctrHoExecutedTotal}} \times 100$ | Handovers sent to a non-strongest target by scoring (%) |

## Performance Counters

The following counters are collected per cell to support KPI calculation and troubleshooting:

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
| `ctrHoTriggeredDeferable` | Deferrable handover events triggered | $0 \text{ to } 2^{31}$ | int64 |
| `ctrHoDeferred` | Events actually deferred (HIGH-activity UE) | $0 \text{ to } 2^{31}$ | int64 |
| `ctrHoGapExecuted` | Deferred handovers executed in a traffic gap | $0 \text{ to } 2^{31}$ | int64 |
| `ctrHoEscalated` | Deferred handovers escalated by RSRP floor | $0 \text{ to } 2^{31}$ | int64 |
| `ctrHoDeferredFail` | Deferred handovers that failed | $0 \text{ to } 2^{31}$ | int64 |
| `ctrHoScoredRedirect` | Handovers redirected to a scored (non-strongest) target | $0 \text{ to } 2^{31}$ | int64 |
| `ctrHoExecutedTotal` | Total handovers executed on the cell | $0 \text{ to } 2^{31}$ | int64 |

## Analysis and Tuning Guidelines

* **Escalation Analysis:** The counter `ctrHoEscalated` acts as a safety-valve indicator and requires regular monitoring. It should remain a small, stable fraction of total deferrals. Sudden step changes in this counter typically indicate neighbor-cell coverage changes rather than parameter drift.
* **High Escalation Ratio:** A high Escalation Ratio indicates that UEs are routinely hitting the RSRP floor during the deferral period. To address this, consider reducing the deferral bounds (e.g., `deferOffset` or `deferTtt`) or raising the `criticalRsrpFloor` parameter.
* **Aggressive Deferral Bounds:** If the `Deferred HO Failure` rate exceeds the cell's overall handover failure rate by more than a factor of approximately 1.5, the deferral bounds are too aggressive for the cell's edge geometry. These bounds must be tightened before attempting any other parameter tuning.
* **Scoring and Redirection:** Analyzing `ctrHoScoredRedirect` in combination with `ctrHoDeferredFail` helps determine whether load-based redirection is introducing an acceptable success-rate cost in exchange for its throughput benefits.

# Cross-References

* [Parameters](parameters.md) — For details on tuning parameters such as `deferOffset`, `deferTtt`, and `criticalRsrpFloor`.
* [Feature Operation](feature-operation.md) — For details on how deferrals, traffic gaps, and RSRP floor escalations are handled operationally.
