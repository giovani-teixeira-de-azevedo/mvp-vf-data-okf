---
type: concept
resource: data/vodafone-mvp/raw/Long Fronthaul over eCPRI.pdf#performance-management
title: Performance Management
description: Performance monitoring guidelines, KPIs, and hardware-timestamped counters
  for validating eCPRI fronthaul delay, jitter, and buffer integrity.
tags:
- eCPRI
- Performance Management
- KPI
- Fronthaul
- Delay
- Jitter
- Counters
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:45:11+00:00'
  source_sha256: a569f16dea1824f5
sources:
- title: Long Fronthaul over eCPRI
  resource: data/vodafone-mvp/raw/Long Fronthaul over eCPRI.pdf
---

# Performance Management

Performance management for Long Fronthaul over eCPRI focuses on monitoring whether the fronthaul contract is holding. This includes checking if delay remains stable inside the budget, jitter is within tolerance, and buffer overruns or underruns are occurring (the direct symptom of a violated contract and the precursor of air-interface errors).

Counters accumulate per fronthaul port per 15-minute Report Output Period (ROP). Because propagation delay is physically fixed, establishing a baseline of the delay per path at integration is recommended; a healthy time series is a flat line, making small anomalies conspicuous.

## Key Performance Indicators (KPIs)

KPIs are designed to track latency budget headroom, buffer health, jitter compliance, and drift frequency.

| KPI | Formula / Counter | Description |
| :--- | :--- | :--- |
| **Delay Margin** | `(maxOneWayDelay - ctrPathDelayMax / 1000) / maxOneWayDelay * 100` | Remaining budget headroom (%) |
| **Buffer Integrity** | `(1 - (ctrBufferUnderruns + ctrBufferOverruns) / ctrEcpriPacketsIn) * 100` | Share of traffic handled without buffer violation (%) |
| **Jitter Compliance** | `ctrJitterViolations` | PDV excursions beyond `jitterBudget` per ROP (target 0) |
| **Drift Event Rate** | `ctrDelayDriftEvents` | Re-validations triggered by delay drift per ROP |

### KPI Operational Guidance

*   **Delay Margin**: This is the headline metric. It should stay above 10% at all times, including on the protection path. It is recommended to run a deliberate protection switch during acceptance testing and record the margin.
*   **Buffer Integrity**: This must be 100%. Any underrun is a discrete air-interface impairment event (lost symbols) and warrants immediate correlation with `ctrDelayDriftEvents` and transport alarms.
*   **Drift Event Rate**: A rising Drift Event rate with constant delay endpoints usually indicates an unstable optical protection state flapping between paths.

## Counters

The delay counters are hardware-timestamped and nanosecond-resolved. 

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
| `ctrPathDelayMin` | Minimum measured one-way delay per ROP | 0–$2^{31}$ ns | int64 |
| `ctrPathDelayMax` | Maximum measured one-way delay per ROP | 0–$2^{31}$ ns | int64 |
| `ctrDelayDriftEvents` | Drift beyond tolerance triggering re-validation | 0–$2^{31}$ | int64 |
| `ctrJitterViolations` | PDV samples beyond `jitterBudget` | 0–$2^{31}$ | int64 |
| `ctrBufferUnderruns` | DL buffer underruns at the radio | 0–$2^{31}$ | int64 |
| `ctrBufferOverruns` | Buffer overruns (late discard) | 0–$2^{31}$ | int64 |
| `ctrEcpriPacketsIn` | eCPRI user-plane packets received per ROP | 0–$2^{63}$ | int64 |
| `ctrHarqBudgetFail` | Validation failures (budget exceeded) | 0–$2^{31}$ | int64 |

### Counter Operational Guidance

*   **`ctrBufferUnderruns`**: Deserves special attention. Even a single underrun per ROP at busy hour indicates that the `bufferHeadroom` is consuming its margin and should be raised before users notice BLER (Block Error Rate) excursions.
*   **`ctrHarqBudgetFail`**: Counts validation failures where the HARQ budget is exceeded. This is normally only observed during transport incidents.

# Cross-References

*   [Parameters](parameters.md) — Configuration parameters such as `maxOneWayDelay`, `jitterBudget`, and `bufferHeadroom` which define the boundaries for these performance measurements.
*   [Feature Operation](feature-operation.md) — Technical details of how measurements and validations are performed within the network.
