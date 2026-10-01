---
type: concept
resource: data/vodafone-mvp/raw/NR Data-Aware Mobility.pdf#performance-management
title: PERFORMANCE MANAGEMENT
description: Performance management guidelines, KPIs, and counters for monitoring
  NR Data-Aware Mobility.
tags:
- Performance Management
- KPIs
- Counters
- NR Data-Aware Mobility
- RAN
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T09:07:58+00:00'
  source_sha256: 3cbc917c8ae5ccd4
sources:
- resource: data/vodafone-mvp/raw/NR Data-Aware Mobility.pdf
  title: NR Data-Aware Mobility
---

This section outlines the performance management framework for the NR Data-Aware Mobility feature. It defines the key performance indicators (KPIs), performance counters, baselining recommendations, and troubleshooting guidelines for monitoring and optimizing the feature's operation.

## Baselining and Monitoring Guidelines

To evaluate the performance and impact of the NR Data-Aware Mobility feature, a baseline of the cell's performance should be established before activation and compared over a monitoring period of at least two weeks. The baseline should include:
* Cell handover success rate
* Ping-pong rate
* Per-UE throughput-during-mobility

All performance counters accumulate per cell over a 15-minute Reporting Period (ROP).

## Key Performance Indicators (KPIs)

The following KPIs are used to assess the efficiency, risk, and value added by the feature's deferral and scoring mechanisms.

| KPI | Formula | Description |
| :--- | :--- | :--- |
| **Deferral Ratio** | $\frac{\text{ctrHoDeferred}}{\text{ctrHoTriggeredDeferable}} \times 100$ | Share of deferrable handovers actually deferred (%) |
| **Gap Execution Ratio** | $\frac{\text{ctrHoGapExecuted}}{\text{ctrHoDeferred}} \times 100$ | Deferred handovers executed inside a traffic gap (%) |
| **Escalation Ratio** | $\frac{\text{ctrHoEscalated}}{\text{ctrHoDeferred}} \times 100$ | Deferred handovers escalated by the RSRP floor (%) |
| **Deferred HO Failure** | $\frac{\text{ctrHoDeferredFail}}{\text{ctrHoDeferred}} \times 100$ | Failure rate of deferred handovers (%) |
| **Scoring Redirect Share** | $\frac{\text{ctrHoScoredRedirect}}{\text{ctrHoExecutedTotal}} \times 100$ | Handovers sent to a non-strongest target by scoring (%) |

### KPI Analysis and Troubleshooting

* **Healthy Configuration Targets:** A healthy configuration typically exhibits a **Gap Execution Ratio** of 40–70% and an **Escalation Ratio** below 5%.
* **High Escalation Ratio:** A high Escalation Ratio indicates that UEs are routinely hitting the RSRP floor during the deferral period. To address this, reduce the `deferOffset` / `deferTtt` parameters or raise the `criticalRsrpFloor` review.
* **Deferred HO Failure:** If the **Deferred HO Failure** rate exceeds the cell's overall handover failure rate by more than a factor of ~1.5, the deferral bounds are too aggressive for the cell's edge geometry. These bounds should be tightened before tuning other parameters.

## Performance Counters

The following counters are accumulated per cell to support the KPIs and provide visibility into the feature's behavior.

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
| `ctrHoTriggeredDeferable` | Deferrable handover events triggered | 0–2³¹ | int64 |
| `ctrHoDeferred` | Events actually deferred (HIGH-activity UE) | 0–2³¹ | int64 |
| `ctrHoGapExecuted` | Deferred handovers executed in a traffic gap | 0–2³¹ | int64 |
| `ctrHoEscalated` | Deferred handovers escalated by RSRP floor | 0–2³¹ | int64 |
| `ctrHoDeferredFail` | Deferred handovers that failed | 0–2³¹ | int64 |
| `ctrHoScoredRedirect` | Handovers redirected to a scored (non-strongest) target | 0–2³¹ | int64 |
| `ctrHoExecutedTotal` | Total handovers executed on the cell | 0–2³¹ | int64 |

### Counter Analysis Guidelines

* **`ctrHoEscalated`:** This acts as a safety-valve counter and requires continuous monitoring. It should remain a small, stable fraction of total deferrals. Step changes in this counter typically indicate neighbor-cell coverage changes rather than parameter drift.
* **`ctrHoScoredRedirect` paired with `ctrHoDeferredFail`:** Analyzing these two counters together helps determine whether load-based redirection is trading a measurable success-rate cost for its throughput benefits.

# Cross-References

* [Feature Operation](feature-operation.md) — For details on how deferrals, traffic gaps, and scoring are executed.
* [Parameters](parameters.md) — For details on parameters such as `deferOffset`, `deferTtt`, and `criticalRsrpFloor`.
