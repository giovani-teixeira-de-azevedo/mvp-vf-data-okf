---
type: concept
resource: data/vodafone-mvp/raw/Flexible PDCCH Monitoring.pdf#performance-management
title: PERFORMANCE MANAGEMENT
description: Details performance management KPIs, counters, and monitoring strategy
  for Flexible PDCCH Monitoring.
tags:
- performance-management
- kpi
- counters
- flexible-pdcch-monitoring
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-30T14:20:01+00:00'
  source_sha256: 2d83f7dc488950ac
sources:
- title: Flexible PDCCH Monitoring
  resource: data/vodafone-mvp/raw/Flexible PDCCH Monitoring.pdf
---

Performance management for Flexible PDCCH Monitoring evaluates the reduction in PDCCH monitoring (serving as a UE energy proxy) alongside the latency impact at traffic burst start. All counters accumulate on a per-cell basis over a 15-minute Result Output Period (ROP).

## Monitoring Strategy

Because UE battery consumption cannot be observed directly, performance management relies on a proxy-based strategy:
* **Energy proxy**: Monitor `ctrSparseTime` and `ctrSkipTime` counters.
* **Latency guarding**: Guard first-packet latency against a two-week pre-activation baseline evaluated over matching hours.

## Key Performance Indicators (KPIs)

The following KPIs are defined for evaluating feature health and efficiency:

| KPI | Formula | Description |
| --- | --- | --- |
| **Sparse Monitoring Share** | `ctrSparseTime / ctrEligibleConnTime × 100` | Headline proxy representing eligible UE connected time in the sparse group (%). Expect 60–85% on smartphone-dominated cells. |
| **Skip Utilization** | `ctrSkipTime / ctrEligibleConnTime × 100` | Eligible UE connected time under active skip (%). Low values are normal on cells without VoNR traffic. |
| **Switch Rate** | `ctrSssgSwitches / (ctrEligibleConnTime / 60000)` | Configuration health indicator measuring group switches per eligible UE-minute. Rates above ~30 switches/UE-min indicate `sssgSwitchTimer` is shorter than the natural burst gap, causing ping-ponging. |
| **Eligible Population** | `ctrEligibleConnTime / ctrConnUeTime × 100` | Share of connected time from capable UEs (%). |

## Performance Counters

When interpreting counters, consideration must be given to UE capability and latency trade-offs:
* **Eligibility split**: On cells with few 3GPP Release 16/17 UEs, a low absolute sparse time is a population artifact rather than a feature fault. Always evaluate `ctrSparseTime` relative to `ctrEligibleConnTime`.
* **Cost-side tracking**: `ctrLateFirstPacket` tracks first packets delayed waiting for a sparse occasion. Its rate should align with the Sparse Monitoring Share and traffic burst arrival rate. A disproportionate rise indicates interaction with a latency-sensitive service that should be repinned to `FULL` via `profile5qiMap`.

| Counter | Description | Range | Datatype |
| --- | --- | --- | --- |
| `ctrConnUeTime` | Aggregated UE RRC-connected time (ms) | 0–2³¹ | `int64` |
| `ctrEligibleConnTime` | Connected time of SSSG/skip-capable UEs (ms) | 0–2³¹ | `int64` |
| `ctrSparseTime` | Eligible UE time in sparse group (ms) | 0–2³¹ | `int64` |
| `ctrSkipTime` | Eligible UE time under skip indication (ms) | 0–2³¹ | `int64` |
| `ctrSssgSwitches` | Search space set group switch indications sent | 0–2³¹ | `int64` |
| `ctrSkipIndications` | Skip indications sent | 0–2³¹ | `int64` |
| `ctrLateFirstPacket` | First packets delayed by a sparse occasion wait | 0–2³¹ | `int64` |

# Cross-References

* [Parameters](parameters.md) — Describes parameters such as `sssgSwitchTimer` and `profile5qiMap` referenced during performance tuning.
