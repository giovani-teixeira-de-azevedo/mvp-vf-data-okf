---
type: concept
resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf#performance-management
title: Performance Management
description: Defines performance management strategy, KPIs, formulas, and counters
  for monitoring Energy-Optimized Symbol Allocation.
tags:
- performance-management
- kpis
- counters
- energy-optimization
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-30T17:16:00+00:00'
  source_sha256: 3c1e203cedc85e02
sources:
- title: Energy-Optimized Symbol Allocation
  resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf
---

This section outlines the performance management strategy, Key Performance Indicators (KPIs), and performance counters for monitoring the Energy-Optimized Symbol Allocation feature.

Performance management answers two questions: how many symbols the feature liberates for muting, and whether the trade of frequency for time costs anything in link performance. The monitoring strategy compares occupied-symbol statistics and standard link KPIs (BLER, throughput) against a two-week pre-activation baseline over matching hours. All counters accumulate per cell over the 15-minute ROP.

## Key Performance Indicators (KPIs)

- **Symbol Occupancy** is the headline KPI: on lightly loaded cells, expect the mean occupied symbols per active slot to drop from 12–13 to 6–8 after activation.
- **Compaction Ratio** shows how often the compact variant wins; values below 20% at low load suggest the frequency widening is blocked — check `prbWideningLimit` and BWP configurations.
- **Compaction BLER** must remain at parity with the overall cell BLER; a gap larger than 0.5 percentage points indicates frequency-selective channels where widening hurts, and `compactionLoadThr` should be lowered.

| KPI | Formula | Description |
| --- | --- | --- |
| **Symbol Occupancy** | `ctrOccupiedSymbols / ctrActiveDlSlots` | Mean occupied symbols per active DL slot |
| **Compaction Ratio** | `ctrCompactedAllocs / ctrPdschAllocs × 100` | Share of PDSCH allocations that were compacted (%) |
| **Muted Symbol Yield** | `ctrMutedSymbols / (ctrActiveDlSlots × 14) × 100` | Share of symbol resource muted in active slots (%) |
| **Compaction BLER** | `ctrCompactedNack / (ctrCompactedNack + ctrCompactedAck) × 100` | HARQ NACK rate on compacted allocations (%) |

## Performance Counters

The ACK/NACK pair for compacted allocations exists specifically to isolate the link performance of the compacted population from the cell aggregate — this is the pair to watch during the pilot phase. `ctrCompactionSuspends` counts guard-triggered suspensions and should track the daily load curve.

| Counter | Description | Range | Datatype |
| --- | --- | --- | --- |
| `ctrActiveDlSlots` | DL slots with at least one transmission | 0–2³¹ | int64 |
| `ctrOccupiedSymbols` | Occupied symbols summed over active DL slots | 0–2³¹ | int64 |
| `ctrMutedSymbols` | Symbols muted by the radio in active slots | 0–2³¹ | int64 |
| `ctrPdschAllocs` | PDSCH allocations scheduled | 0–2³¹ | int64 |
| `ctrCompactedAllocs` | Allocations scheduled in compacted form | 0–2³¹ | int64 |
| `ctrCompactedAck` | HARQ ACKs on compacted allocations | 0–2³¹ | int64 |
| `ctrCompactedNack` | HARQ NACKs on compacted allocations | 0–2³¹ | int64 |
| `ctrCompactionSuspends` | Compaction suspensions due to load/user guards | 0–2³¹ | int64 |

# Cross-References

- [Parameters](parameters.md) — Configuration parameters such as `prbWideningLimit` and `compactionLoadThr` that influence compaction behavior.
