---
type: procedure
resource: data/vodafone-mvp/raw/Energy-Optimized Slot Allocation.pdf#deactivation-procedure
title: DEACTIVATION PROCEDURE
description: Procedure to deactivate the Energy-Optimized Slot Allocation feature
  and revert to default scheduler packing.
tags:
- deactivation
- rancli
- energy-saving
- slot-allocation
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:40:28+00:00'
  source_sha256: 4e97c5715fa3f3f3
sources:
- title: Energy-Optimized Slot Allocation
  resource: data/vodafone-mvp/raw/Energy-Optimized Slot Allocation.pdf
---

This section details the deactivation procedure for the Energy-Optimized Slot Allocation feature, including traffic impact considerations and step-by-step command-line instructions.

## Traffic Impact and System Behavior

Deactivating the feature has no negative impact on user traffic:
- **Buffer Flushing:** Upon deactivation, queued batches are flushed immediately and the scheduler reverts to default packing at the next scheduling occasion.
- **Latency:** Latency returns to baseline instantly.
- **Micro-sleep Savings:** The only observable effect is a drop in the Empty Slot Ratio and, consequently, in micro-sleep savings.

> **Note:** Leave **NR Micro Sleep Tx** active so that the system continues to exploit whatever empty slots occur naturally.

## Deactivation Procedure

The deactivation is performed in three steps using the `rancli` interface:

### Step 1: Disable Per Cell (Flushes Batching Buffers)
Disable batching on the specific cell to flush the buffers.

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A slotAllocMode=DISABLED
```

### Step 2: Deactivate the Feature
Turn off the feature control state.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=EnergyOptSlotAlloc featureState=DEACTIVATED
```

### Step 3: Verify Deactivation
Verify that the allocation mode has been disabled and verify that the counter `ctrBatchedPackets` stops incrementing in the subsequent Recording Observation Period (ROP).

```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A slotAllocMode
```

# Cross-References

- [Activation Procedure](activation-procedure.md)
- [Performance Management](performance-management.md)
