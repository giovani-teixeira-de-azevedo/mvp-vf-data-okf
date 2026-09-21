---
type: concept
resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf#performance-management
title: Performance Management
description: Details the performance management strategy, KPIs, and PM counters for
  monitoring the Energy-Optimized Symbol Allocation feature.
tags:
- energy-saving
- performance-management
- kpi
- pm-counters
- symbol-allocation
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:41:03+00:00'
  source_sha256: 3c1e203cedc85e02
sources:
- title: Energy-Optimized Symbol Allocation
  resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf
---

This section outlines the performance management strategy for the Energy-Optimized Symbol Allocation feature, including the principal Key Performance Indicators (KPIs) and the performance counters used to evaluate energy-saving and link performance.

## Monitoring Strategy

Performance management answers two primary questions:
1. How many symbols the feature liberates for muting.
2. Whether the trade-off of frequency for time impacts link performance.

The monitoring strategy compares occupied-symbol statistics and standard link KPIs (such as BLER and throughput) against a two-week pre-activation baseline over matching hours. All performance counters accumulate per cell over a 15-minute Result Output Period (ROP).

## Key Performance Indicators (KPIs)

The following KPIs are used to monitor the feature's behavior and efficacy:

*   **Symbol Occupancy:** This is the headline KPI. On lightly loaded cells, the mean occupied symbols per active slot are expected to drop from 12–13 symbols to 6–8 symbols after activation.
*   **Compaction Ratio:** This metric shows how often the compact variant is selected. Values below 20% at low load suggest that frequency widening is blocked; in such cases, verify the `prbWideningLimit` and BWP configurations.
*   **Compaction BLER:** This must remain at parity with the overall cell BLER. A gap larger than 0.5 percentage points indicates frequency-selective channels where widening is detrimental; under these conditions, `compactionLoadThr` should be lowered.

### KPI Formulas

| KPI | Formula | Description |
| :--- | :--- | :--- |
| **Symbol Occupancy** | `ctrOccupiedSymbols / ctrActiveDlSlots` | Mean occupied symbols per active DL slot |
| **Compaction Ratio** | `(ctrCompactedAllocs / ctrPdschAllocs) * 100` | Share of PDSCH allocations that were compacted (%) |
| **Muted Symbol Yield** | `ctrMutedSymbols / (ctrActiveDlSlots * 14) * 100` | Share of symbol resource muted in active slots (%) |
| **Compaction BLER** | `ctrCompactedNack / (ctrCompactedNack + ctrCompactedAck) * 100` | HARQ NACK rate on compacted allocations (%) |

## Performance Counters

The performance counter pair for ACK/NACK on compacted allocations exists specifically to isolate the link performance of the compacted population from the cell aggregate. This pair is crucial to monitor during the pilot phase. Additionally, `ctrCompactionSuspends` tracks guard-triggered suspensions and is expected to follow the daily load curve.

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
| `ctrActiveDlSlots` | DL slots with at least one transmission | 0–2^31 | int64 |
| `ctrOccupiedSymbols` | Occupied symbols summed over active DL slots | 0–2^31 | int64 |
| `ctrMutedSymbols` | Symbols muted by the radio in active slots | 0–2^31 | int64 |
| `ctrPdschAllocs` | PDSCH allocations scheduled | 0–2^31 | int64 |
| `ctrCompactedAllocs` | Allocations scheduled in compacted form | 0–2^31 | int64 |
| `ctrCompactedAck` | HARQ ACKs on compacted allocations | 0–2^31 | int64 |
| `ctrCompactedNack` | HARQ NACKs on compacted allocations | 0–2^31 | int64 |
| `ctrCompactionSuspends` | Compaction suspensions due to load/user guards | 0–2^31 | int64 |

# Cross-References

- [Parameters](parameters.md) — For configuration parameters such as `prbWideningLimit` and `compactionLoadThr`.
- [Feature Operation](feature-operation.md) — For slot compaction mechanisms and guard thresholds.
