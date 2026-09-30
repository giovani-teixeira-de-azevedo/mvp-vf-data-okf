---
type: procedure
resource: data/vodafone-mvp/raw/Energy-Optimized Slot Allocation.pdf#deactivation-procedure
title: Deactivation Procedure
description: Details the step-by-step deactivation procedure and traffic impact for
  Energy-Optimized Slot Allocation.
tags:
- energy-saving
- slot-allocation
- deactivation
- rancli
- nr-cell
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-30T09:38:47+00:00'
  source_sha256: 4e97c5715fa3f3f3
sources:
- resource: data/vodafone-mvp/raw/Energy-Optimized Slot Allocation.pdf
  title: Energy-Optimized Slot Allocation
---

This section outlines the procedure for deactivating the Energy-Optimized Slot Allocation feature, detailing traffic impact, operational behavior, and the required execution steps.

## Operational Impact and Behavior

Deactivating the feature results in no traffic impact:
- **Buffer Flushing & Scheduling:** Queued batches are flushed immediately, and the scheduler reverts to default packing at the next scheduling occasion.
- **Latency:** Latency returns to baseline instantly.
- **Micro-Sleep Impact:** The only observable effect is a drop in the Empty Slot Ratio and a corresponding reduction in micro-sleep savings.
- **NR Micro Sleep Tx:** Leave NR Micro Sleep Tx active so that it continues to exploit naturally occurring empty slots.

## Execution Steps

1. **Disable per cell** (flushes batching buffers):
   ```bash
   rancli set NodeRoot=1,NrFunction=1,NrCell=N1A slotAllocMode=DISABLED
   ```

2. **Deactivate the feature control**:
   ```bash
   rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=EnergyOptSlotAlloc featureState=DEACTIVATED
   ```

3. **Verify status**:
   ```bash
   rancli get NodeRoot=1,NrFunction=1,NrCell=N1A slotAllocMode
   ```
   Confirm the mode setting and verify that `ctrBatchedPackets` stops incrementing in the following Result Output Period (ROP).

## Cross-References

- [Activation Procedure](activation-procedure.md)
- [Performance Management](performance-management.md)
