---
type: concept
resource: data/vodafone-mvp/raw/Energy-Optimized Slot Allocation.pdf#performance-management
title: Performance Management
description: Details performance metrics, KPIs, formulas, and counters for evaluating
  the Energy-Optimized Slot Allocation feature over 15-minute reporting periods.
tags:
- performance-management
- kpi
- counters
- energy-saving
- slot-allocation
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-30T09:38:32+00:00'
  source_sha256: cd127218a5ec6765
sources:
- title: Energy-Optimized Slot Allocation
  resource: data/vodafone-mvp/raw/Energy-Optimized Slot Allocation.pdf
---

Performance management evaluates both the enabler metric (how many sleep-capable empty slots the feature creates) and the cost metric (the latency price paid by connected users) for the Energy-Optimized Slot Allocation feature. All counters accumulate per cell over a 15-minute Result Output Period (ROP) and are evaluated against a two-week pre-activation baseline of empty-slot ratio, scheduling latency, and radio energy over matching hours.

## Key Performance Indicators (KPIs)

Performance evaluation relies on four key metrics:

* **Empty Slot Ratio**: The headline enabler KPI. Night-time values are expected to exceed 80% on residential cells with a clear step increase versus baseline.
* **Batch Efficiency**: Shows how full the occupied slots are. Values below 60% indicate that the delay bound is triggering before the fill target, meaning traffic is either too sparse to batch (harmless) or `targetSlotFill` is set unrealistically high.
* **Added Delay**: Quantifies queuing latency added by batching. Delay must remain within the configured bound; if the P95 latency approaches `maxBatchDelay` at moderate load, `batchingAllowedLoad` should be reduced.
* **Sleep Window Yield**: Measures consecutive empty slot windows per empty-slot pair. Higher values indicate better slot clustering.

| KPI Formula Description | Formula | Description |
| --- | --- | --- |
| Empty Slot Ratio | `ctrEmptyDlSlots / ctrDlSlots × 100` | Share of DL slots with no transmission (%) |
| Batch Efficiency | `ctrBatchedPrbUsed / ctrBatchedPrbAvail × 100` | Average fill of batch-scheduled slots (%) |
| Added Delay | `ctrBatchDelaySum / ctrBatchedPackets` | Mean added queuing delay per batched packet (ms) |
| Sleep Window Yield | `ctrSleepWindows / (ctrEmptyDlSlots / 2)` | Consecutive-empty-slot windows per empty-slot pair; higher means better clustering |

## Counters

Slot counters provide the raw material for enabler KPIs, while `ctrBatchDelaySum` and `ctrBatchedPackets` quantify latency cost. Special attention should be paid to `ctrBatchSuspends`, which counts automatic suspensions due to load thresholds and should mirror the daily traffic curve. Suspensions during night hours suggest a load-measurement anomaly or misconfigured `batchingAllowedLoad`.

| Counter | Description | Range | Datatype |
| --- | --- | --- | --- |
| `ctrDlSlots` | Downlink slots in the ROP | 0–2³¹ | int64 |
| `ctrEmptyDlSlots` | Downlink slots with no transmission | 0–2³¹ | int64 |
| `ctrSleepWindows` | Windows of ≥ `minSleepWindow` consecutive empty slots | 0–2³¹ | int64 |
| `ctrBatchedPackets` | Packets scheduled via batching | 0–2³¹ | int64 |
| `ctrBatchDelaySum` | Accumulated added queuing delay (ms) | 0–2³¹ | int64 |
| `ctrBatchedPrbUsed` | PRBs used in batch-scheduled slots | 0–2³¹ | int64 |
| `ctrBatchedPrbAvail` | PRBs available in batch-scheduled slots | 0–2³¹ | int64 |
| `ctrBatchSuspends` | Batching suspensions due to load threshold | 0–2³¹ | int64 |

# Cross-References

* [Parameters](parameters.md) — Configuration parameters controlling feature thresholds, fill targets, and delay bounds.
