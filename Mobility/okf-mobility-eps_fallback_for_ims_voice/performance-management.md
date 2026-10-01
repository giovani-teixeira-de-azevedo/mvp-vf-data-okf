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
  at: '2026-10-01T11:01:31+00:00'
  source_sha256: cb4d29330fd6159e
sources:
- resource: data/vodafone-mvp/raw/EPS Fallback for IMS Voice.pdf
  title: EPS Fallback for IMS Voice
---

This section outlines the performance management framework for EPS Fallback for IMS Voice. It defines the key performance indicators (KPIs) and performance counters used to monitor fallback success rates, setup delays, and target selection health.

## Monitoring Guidelines

To ensure a reliable transition, establish a baseline of IMS call setup success and setup time from the LTE layer before enabling Standalone (SA) voice traffic. Once enabled, monitor the KPIs daily during the first two weeks. All counters accumulate per cell over a 15-minute Reporting Period (ROP).

Performance management answers three primary questions:
1. **Are voice calls successfully reaching EPS?** (Fallback Success Rate)
2. **How fast is the fallback?** (Setup delay contribution, split by method)
3. **Is target selection healthy?** (Share of measurement-based versus blind fallbacks, and post-fallback failures)

---

## Key Performance Indicators (KPIs)

The KPIs are derived from the performance counters. 

* **Fallback Success Rate**: Should exceed 99% in a mature network. Values of 95–99% usually indicate stale LTE target data or coverage holes on the configured EARFCNs. In this case, check which target frequencies dominate `ctrEpsFbFail` in per-frequency PM breakdowns.
* **Measured Target Ratio**: Should be close to 100% when `fallbackMeasEnabled` is set to `true`. A low value indicates that UEs are not finding any LTE carrier above `b1ThresholdRsrp` before the timer expires. This can be resolved by lowering the threshold or extending `fallbackMeasTimer`.
* **Blind Redirect Share**: Serves as a risk indicator, as each blind redirect is a candidate for post-fallback setup failure.

### KPI Formulas

| KPI | Formula | Description |
| :--- | :--- | :--- |
| **Fallback Success Rate** | $\frac{\text{ctrEpsFbSuccess}}{\text{ctrEpsFbAttempt}} \times 100$ | Share of triggered fallbacks completing with UE confirmed on EPS (%) |
| **Measured Target Ratio** | $\frac{\text{ctrEpsFbMeasBased}}{\text{ctrEpsFbAttempt}} \times 100$ | Share of fallbacks using a B1-reported target (%) |
| **Blind Redirect Share** | $\frac{\text{ctrEpsFbAttempt} - \text{ctrEpsFbMeasBased}}{\text{ctrEpsFbAttempt}} \times 100$ | Share of fallbacks executed blind (%) |
| **Handover Method Share** | $\frac{\text{ctrEpsFbHoExec}}{\text{ctrEpsFbAttempt}} \times 100$ | Share of fallbacks using the handover method (%) |
| **Mean Fallback Delay** | $\frac{\text{ctrEpsFbDelaySum}}{\text{ctrEpsFbSuccess}}$ | Average time from trigger to completion (ms) |

---

## Performance Counters

The counters record the fallback pipeline stage by stage. 

* **ctrEpsFbFail**: Warrants attention on its own. It increments when the guard timer expires without handover confirmation or when the handover preparation is rejected. A sudden rise typically maps to an N26 interface or MME-side change rather than a radio link problem.
* **ctrEpsFbDelaySum**: A sum-of-durations counter paired with `ctrEpsFbSuccess` to calculate the average fallback delay.

### Counter Definitions

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
| **ctrEpsFbAttempt** | Fallback procedures triggered (5QI 1 flow rejected with fallback cause) | $0 - 2^{31}$ | int64 |
| **ctrEpsFbSuccess** | Fallbacks completed successfully | $0 - 2^{31}$ | int64 |
| **ctrEpsFbFail** | Fallbacks failed (guard timer expiry or preparation reject) | $0 - 2^{31}$ | int64 |
| **ctrEpsFbMeasBased** | Fallbacks executed toward a B1-measured target | $0 - 2^{31}$ | int64 |
| **ctrEpsFbHoExec** | Fallbacks executed with the handover method | $0 - 2^{31}$ | int64 |
| **ctrEpsFbRedirectExec** | Fallbacks executed with the redirect method | $0 - 2^{31}$ | int64 |
| **ctrEpsFbDelaySum** | Accumulated trigger-to-completion time (ms) | $0 - 2^{31}$ | int64 |

# Cross-References

* [Parameters](parameters.md) — For details on parameters such as `fallbackMeasEnabled`, `b1ThresholdRsrp`, and `fallbackMeasTimer`.
* [Feature Operation](feature-operation.md) — For details on the fallback execution methods (handover vs. redirection).
* [Activation Procedure](activation-procedure.md) — For details on enabling the feature in the network.
