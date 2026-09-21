---
type: reference-table
resource: data/vodafone-mvp/raw/Energy-Optimized Slot Allocation.pdf#parameters
title: PARAMETERS
description: Parameters and configuration knobs for Energy-Optimized Slot Allocation,
  including default values, datatypes, and operational ranges.
tags:
- Energy-Optimized Slot Allocation
- Configuration Parameters
- 5G RAN Energy Saving
- QoS
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:40:42+00:00'
  source_sha256: 6f645014b6dc442a
sources:
- resource: data/vodafone-mvp/raw/Energy-Optimized Slot Allocation.pdf
  title: Energy-Optimized Slot Allocation
---

This section details the configuration parameters and tuning knobs for the Energy-Optimized Slot Allocation feature. These parameters control latency trade-offs, packing thresholds, and load limits to balance energy savings with user experience.

### Tuning Considerations

The two central knobs are `maxBatchDelay` (the latency price) and `targetSlotFill` (the packing ambition). Raising the fill target beyond 90% yields diminishing sleep gains while increasing the chance of delay-bound-triggered fragmentation. The default values are recommended for most deployments.

### Parameter Reference

| Parameter | Description | Values | Datatype | Default |
| :--- | :--- | :--- | :--- | :--- |
| `slotAllocMode` | Enables the function on the cell | `DISABLED`, `ENABLED` | enum | `DISABLED` |
| `maxBatchDelay` | Maximum added queuing delay for batched traffic | 1–8 (ms) | int32 | 4 |
| `targetSlotFill` | Slot fill level that triggers batch scheduling | 50–100 (%) | int32 | 85 |
| `batchingAllowedLoad` | PRB utilization above which batching is suspended | 10–100 (%) | int32 | 40 |
| `exempt5qiList` | 5QIs exempt from batching (in addition to 5QI 1, 82–85) | list of 5QI | string | `"2,65,66"` |
| `minSleepWindow` | Minimum consecutive empty slots worth creating | 1–20 (slots) | int32 | 2 |
| `retxSteering` | Steer HARQ retransmissions into occupied slots | `OFF`, `ON` | enum | `ON` |

# Cross-References

* [FEATURE OVERVIEW](feature-overview.md)
* [FEATURE OPERATION](feature-operation.md)
* [ACTIVATION PROCEDURE](activation-procedure.md)
