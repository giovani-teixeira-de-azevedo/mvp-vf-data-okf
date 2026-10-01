---
type: concept
resource: data/vodafone-mvp/raw/EPS Fallback for IMS Voice.pdf#performance-management
title: Performance Management
description: Performance management guidelines, KPIs, and counters for monitoring
  EPS Fallback for IMS Voice.
tags:
- EPS Fallback
- IMS Voice
- Performance Management
- KPIs
- Counters
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T10:15:00+00:00'
  source_sha256: cb4d29330fd6159e
sources:
- title: EPS Fallback for IMS Voice
  resource: data/vodafone-mvp/raw/EPS Fallback for IMS Voice.pdf
---

This section outlines the performance management framework for EPS Fallback for IMS Voice, including key performance indicators (KPIs) and performance monitoring counters. It details how to assess fallback success rates, setup delays, and target selection health.

## Performance Monitoring Guidelines

Performance management answers three key questions:
1. Are voice calls successfully reaching EPS (fallback success rate)?
2. How fast are they reaching EPS (setup delay contribution, split by method)?
3. Is target selection healthy (share of measurement-based versus blind fallbacks, and post-fallback failures)?

Before enabling Standalone (SA) voice traffic, establish a baseline of IMS call setup success and setup time from the LTE layer. Once enabled, monitor the KPIs below daily during the first two weeks. All counters accumulate per cell over a 15-minute Recording Observation Period (ROP).

## Key Performance Indicators (KPIs)

The KPIs are derived from the counters in the [Performance Counters](#performance-counters) section. 

*   **Fallback Success Rate**: Should exceed 99% in a mature network. Values of 95–99% usually indicate stale LTE target data or coverage holes on the configured EARFCNs. Check which target frequencies dominate `ctrEpsFbFail` in per-frequency PM breakdowns.
*   **Measured Target Ratio**: Should be close to 100% where `fallbackMeasEnabled` is set to true. A low value means UEs are not finding any LTE carrier above `b1ThresholdRsrp` before the timer expires. In this case, lower the threshold or extend `fallbackMeasTimer` (see [Parameters](parameters.md)).
*   **Blind Redirect Share**: This is a risk indicator, as each blind redirect is a candidate for post-fallback setup failure.

### KPI Formulas

| KPI | Formula | Description |
| :--- | :--- | :--- |
| **Fallback Success Rate** | `ctrEpsFbSuccess / ctrEpsFbAttempt × 100` | Share of triggered fallbacks completing with UE confirmed on EPS (%) |
| **Measured Target Ratio** | `ctrEpsFbMeasBased / ctrEpsFbAttempt × 100` | Share of fallbacks using a B1-reported target (%) |
| **Blind Redirect Share** | `(ctrEpsFbAttempt − ctrEpsFbMeasBased) / ctrEpsFbAttempt × 100` | Share of fallbacks executed blind (%) |
| **Handover Method Share** | `ctrEpsFbHoExec / ctrEpsFbAttempt × 100` | Share of fallbacks using the handover method (%) |
| **Mean Fallback Delay** | `ctrEpsFbDelaySum / ctrEpsFbSuccess` | Average time from trigger to completion (ms) |

## Performance Counters

The counters record the fallback pipeline stage by stage. 

*   `ctrEpsFbFail` warrants attention on its own: it increments when the guard timer expires without handover confirmation or when the handover preparation is rejected. A sudden rise typically maps to an N26 or MME-side change rather than a radio problem.
*   `ctrEpsFbDelaySum` is a sum-of-durations counter paired with `ctrEpsFbSuccess` for averaging.

### Counter Definitions

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
| `ctrEpsFbAttempt` | Fallback procedures triggered (5QI 1 flow rejected with fallback cause) | 0–2³¹ | int64 |
| `ctrEpsFbSuccess` | Fallbacks completed successfully | 0–2³¹ | int64 |
| `ctrEpsFbFail` | Fallbacks failed (guard timer expiry or preparation reject) | 0–2³¹ | int64 |
| `ctrEpsFbMeasBased` | Fallbacks executed toward a B1-measured target | 0–2³¹ | int64 |
| `ctrEpsFbHoExec` | Fallbacks executed with the handover method | 0–2³¹ | int64 |
| `ctrEpsFbRedirectExec` | Fallbacks executed with the redirect method | 0–2³¹ | int64 |
| `ctrEpsFbDelaySum` | Accumulated trigger-to-completion time (ms) | 0–2³¹ | int64 |

# Cross-References

* [Parameters](parameters.md)
* [Feature Operation](feature-operation.md)
