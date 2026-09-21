---
type: concept
resource: data/vodafone-mvp/raw/Flexible PDCCH Monitoring.pdf#performance-management
title: Performance Management
description: Covers performance management for Flexible PDCCH Monitoring, including
  key performance indicators (KPIs) and counters used to measure UE energy savings
  and latency impacts.
tags:
- performance-management
- kpis
- counters
- pdcch-monitoring
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:41:36+00:00'
  source_sha256: 2d83f7dc488950ac
sources:
- resource: data/vodafone-mvp/raw/Flexible PDCCH Monitoring.pdf
  title: Flexible PDCCH Monitoring
---

This section outlines the performance management strategy for the Flexible PDCCH Monitoring feature. It details Key Performance Indicators (KPIs) and performance counters used to evaluate energy saving benefits (UE energy proxy) against latency impacts on traffic bursts.

## Strategy Overview

Performance management for Flexible PDCCH Monitoring addresses two critical questions:
1. **UE Energy Savings Proxy:** How much PDCCH monitoring the feature successfully removes.
2. **Latency Price:** The latency penalty introduced at the start of traffic bursts.

Because direct UE battery consumption is not observable by the network, a proxy-based strategy is employed:
* **Energy Savings:** Monitored using sparse-time and skip-time counters.
* **Latency Guard:** First-packet latency is monitored and compared against a two-week pre-activation baseline over matching hours.

All counters accumulate on a per-cell basis over a 15-minute Reporting Observation Period (ROP).

---

## Key Performance Indicators (KPIs)

* **Sparse Monitoring Share:** The headline proxy KPI. For cells dominated by smartphone traffic, the expected value is between **60% and 85%** of eligible UE connected time in the sparse group.
* **Switch Rate:** Indicates configuration health. If the switch rate exceeds approximately 30 switches per UE-minute, it indicates that the timer is shorter than the traffic's natural burst gap, causing the UE to ping-pong between monitoring states. This should be resolved by lengthening [sssgSwitchTimer](parameters.md).
* **Skip Utilization:** Indicates whether the scheduler finds exploitable quiet windows. Low values are normal on cells that do not carry VoNR traffic.
* **Eligible Population:** Reflects the share of connected time belonging to capability-supported UEs.

### KPI Formulae

| KPI | Formula | Description |
| :--- | :--- | :--- |
| **Sparse Monitoring Share** | $\frac{\text{ctrSparseTime}}{\text{ctrEligibleConnTime}} \times 100$ | Eligible UE connected time in the sparse group (%) |
| **Skip Utilization** | $\frac{\text{ctrSkipTime}}{\text{ctrEligibleConnTime}} \times 100$ | Eligible UE connected time under active skip (%) |
| **Switch Rate** | $\frac{\text{ctrSssgSwitches}}{\text{ctrEligibleConnTime} / 60000}$ | Group switches per eligible UE-minute |
| **Eligible Population** | $\frac{\text{ctrEligibleConnTime}}{\text{ctrConnUeTime}} \times 100$ | Share of connected time from capable UEs (%) |

---

## Performance Counters

When analyzing counters, understanding the population eligibility split is critical. On cells with a low population of Release 16/17 UEs, a low absolute sparse time (`ctrSparseTime`) is an artifact of the population mix rather than a feature malfunction. Therefore, `ctrSparseTime` must always be evaluated relative to `ctrEligibleConnTime`.

Special attention should be paid to `ctrLateFirstPacket` as the primary cost-side metric. It tracks first packets whose scheduling was delayed waiting for a sparse monitoring occasion. Under normal operation:
* The rate of delayed packets should scale with the Sparse Monitoring Share and the traffic burst arrival rate.
* A disproportionate rise in this counter indicates a performance interaction with a latency-sensitive service. If this occurs, the affected service profile should be repinned to `FULL` within [profile5qiMap](parameters.md).

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
| `ctrConnUeTime` | Aggregated UE RRC-connected time (ms) | 0–$2^{31}$ | int64 |
| `ctrEligibleConnTime` | Connected time of SSSG/skip-capable UEs (ms) | 0–$2^{31}$ | int64 |
| `ctrSparseTime` | Eligible UE time in sparse group (ms) | 0–$2^{31}$ | int64 |
| `ctrSkipTime` | Eligible UE time under skip indication (ms) | 0–$2^{31}$ | int64 |
| `ctrSssgSwitches` | Search space set group switch indications sent | 0–$2^{31}$ | int64 |
| `ctrSkipIndications` | Skip indications sent | 0–$2^{31}$ | int64 |
| `ctrLateFirstPacket` | First packets delayed by a sparse occasion wait | 0–$2^{31}$ | int64 |

# Cross-References

* [Parameters](parameters.md) — For configuration of `sssgSwitchTimer` and the `profile5qiMap` mapping profile.
* [Feature Operation](feature-operation.md) — For SSSG and skip indication mechanism details.
