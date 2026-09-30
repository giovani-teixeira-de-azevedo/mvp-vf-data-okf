---
type: reference-table
resource: data/vodafone-mvp/raw/Energy-Optimized Slot Allocation.pdf#parameters
title: PARAMETERS
description: Configuration parameters controlling Energy-Optimized Slot Allocation,
  including cell activation, queuing delay, slot fill targets, load thresholds, exempt
  5QIs, sleep windows, and retransmission steering.
tags:
- parameters
- configuration
- energy-saving
- slot-allocation
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-30T09:38:32+00:00'
  source_sha256: 6f645014b6dc442a
sources:
- title: Energy-Optimized Slot Allocation
  resource: data/vodafone-mvp/raw/Energy-Optimized Slot Allocation.pdf
---

This section defines the configuration parameters for Energy-Optimized Slot Allocation.

The two central knobs are `maxBatchDelay` (the latency price) and `targetSlotFill` (the packing ambition). Raising the fill target beyond 90% yields diminishing sleep gains while increasing the chance of delay-bound-triggered fragmentation; the defaults are recommended for most deployments.

## Parameter Summary

| Parameter | Description | Values | Datatype | Default |
| --- | --- | --- | --- | --- |
| `slotAllocMode` | Enables the function on the cell | DISABLED, ENABLED | enum | DISABLED |
| `maxBatchDelay` | Maximum added queuing delay for batched traffic | 1–8 (ms) | int32 | 4 |
| `targetSlotFill` | Slot fill level that triggers batch scheduling | 50–100 (%) | int32 | 85 |
| `batchingAllowedLoad` | PRB utilization above which batching is suspended | 10–100 (%) | int32 | 40 |
| `exempt5qiList` | 5QIs exempt from batching (in addition to 5QI 1, 82–85) | list of 5QI | string | "2,65,66" |
| `minSleepWindow` | Minimum consecutive empty slots worth creating | 1–20 (slots) | int32 | 2 |
| `retxSteering` | Steer HARQ retransmissions into occupied slots | OFF, ON | enum | ON |
