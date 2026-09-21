---
type: concept
resource: data/vodafone-mvp/raw/Energy-Optimized Slot Allocation.pdf#performance-management
title: Performance Management
description: Guidelines, Key Performance Indicators (KPIs), and counters for evaluating
  the energy-saving efficiency and latency impact of the Energy-Optimized Slot Allocation
  feature.
tags:
- performance-management
- kpi
- counters
- energy-optimization
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:40:25+00:00'
  source_sha256: cd127218a5ec6765
sources:
- title: Energy-Optimized Slot Allocation
  resource: data/vodafone-mvp/raw/Energy-Optimized Slot Allocation.pdf
---

Performance management for the Energy-Optimized Slot Allocation feature evaluates two primary aspects: the number of sleep-capable empty slots created (the enabler metric) and the latency price paid by connected users (the cost metric).

Evaluation is performed by comparing performance against a two-week pre-activation baseline of empty-slot ratio, scheduling latency, and radio energy over matching hours. All counters accumulate per cell over a 15-minute Reporting Period (ROP).

## Key Performance Indicators (KPIs)

| KPI | KPI Formula | Description |
| :--- | :--- | :--- |
| **Empty Slot Ratio** | `ctrEmptyDlSlots / ctrDlSlots * 100` | Headline enabler KPI representing the share of downlink slots with no transmission (%). Night-time values are expected to be above 80% on residential cells, showing a clear step versus the baseline. |
| **Batch Efficiency** | `ctrBatchedPrbUsed / ctrBatchedPrbAvail * 100` | Average fill of batch-scheduled slots (%). Values below 60% mean the delay bound is triggering before the fill target, indicating either that traffic is too sparse to batch (harmless) or that `targetSlotFill` is set unrealistically high. |
| **Added Delay** | `ctrBatchDelaySum / ctrBatchedPackets` | Mean added queuing delay per batched packet (ms). This must stay within the configured bound; if the P95 latency approaches `maxBatchDelay` at moderate load, `batchingAllowedLoad` should be reduced. |
| **Sleep Window Yield** | `ctrSleepWindows / (ctrEmptyDlSlots / 2)` | Consecutive-empty-slot windows per empty-slot pair. Higher values indicate better clustering of empty slots. |

## Counters

Slot counters provide the raw material for calculating the enabler KPIs, while `ctrBatchDelaySum` and `ctrBatchedPackets` quantify the latency cost. 

Special attention should be paid to `ctrBatchSuspends`, which counts automatic suspensions due to load. It should follow the daily traffic curve. Suspensions during night hours indicate a load-measurement anomaly or a misconfigured `batchingAllowedLoad`.

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
| `ctrDlSlots` | Downlink slots in the ROP | 0 to $2^{31}$ | `int64` |
| `ctrEmptyDlSlots` | Downlink slots with no transmission | 0 to $2^{31}$ | `int64` |
| `ctrSleepWindows` | Windows of $\ge$ `minSleepWindow` consecutive empty slots | 0 to $2^{31}$ | `int64` |
| `ctrBatchedPackets` | Packets scheduled via batching | 0 to $2^{31}$ | `int64` |
| `ctrBatchDelaySum` | Accumulated added queuing delay (ms) | 0 to $2^{31}$ | `int64` |
| `ctrBatchedPrbUsed` | PRBs used in batch-scheduled slots | 0 to $2^{31}$ | `int64` |
| `ctrBatchedPrbAvail` | PRBs available in batch-scheduled slots | 0 to $2^{31}$ | `int64` |
| `ctrBatchSuspends` | Batching suspensions due to load threshold | 0 to $2^{31}$ | `int64` |

# Cross-References

* [Parameters](parameters.md) — Configuration parameters such as `targetSlotFill`, `maxBatchDelay`, and `batchingAllowedLoad` which influence these performance metrics.
