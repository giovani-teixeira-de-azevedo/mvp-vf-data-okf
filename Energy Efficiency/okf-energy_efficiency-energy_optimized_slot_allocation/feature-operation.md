---
type: concept
resource: data/vodafone-mvp/raw/Energy-Optimized Slot Allocation.pdf#feature-operation
title: FEATURE OPERATION
description: Describes the scheduling mechanisms, batching triggers, micro-sleep signaling,
  and load guard operations for Energy-Optimized Slot Allocation.
tags:
- energy-saving
- slot-allocation
- scheduler
- batching
- micro-sleep
- ran
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-30T09:38:36+00:00'
  source_sha256: 126530ea028e5c8e
sources:
- resource: data/vodafone-mvp/raw/Energy-Optimized Slot Allocation.pdf
  title: Energy-Optimized Slot Allocation
---

This section details the internal operational mechanisms of the Energy-Optimized Slot Allocation feature within the Energy-Optimized Slot Allocation document. It explains packet batching, trigger conditions, scheduling rules, micro-sleep signaling to the radio unit, and load-based suspension.

## Batching and Scheduling Mechanisms

Per cell, the scheduler maintains a batching horizon defined by `maxBatchDelay` milliseconds. Non-exempt downlink data arriving during this horizon is accumulated in per-UE queues instead of being scheduled immediately at the first opportunity.

### Batch Triggers

A batch trigger fires when either of the following conditions is met:
- **Delay Bound Guard:** The oldest queued packet approaches its delay bound minus a scheduling guard.
- **Slot Fill Target:** The accumulated data volume can fill a slot to at least `targetSlotFill` percent of available Physical Resource Blocks (PRBs).

Upon firing a trigger, the scheduler packs the queued data into consecutive slots at maximum fill, and then returns to packet accumulation.

### Handling Exempt Data and Retransmissions

- **Exempt Data:** Critical or delay-sensitive traffic (such as VoNR 5QI 1 packets) bypasses batching and is scheduled immediately.
- **HARQ Retransmissions:** Scheduled immediately due to carrying latency debt, but steered into already-occupied slots where possible.

### Radio Unit Signaling and Micro-Sleep

Empty slots are signaled to the radio unit one slot ahead of time, providing the required advance notice for the micro-sleep function to enter its power-down window (such as Power Amplifier micro-sleep).

## Operational Sequence

```mermaid
sequenceDiagram 
    participant DL as DL Data Arrival 
    participant BUF as Batching Buffer 
    participant SCH as Scheduler 
    participant RU as Radio Unit 
    DL->>BUF: Packets for UE1..UE4 (t = 0..5 ms) 
    BUF->>BUF: Accumulate (delay bound not reached) 
    BUF->>SCH: Trigger: slot fill target reached 
    SCH->>RU: Slot n: 92% filled (UE1..UE4) 
    SCH->>RU: Slots n+1..n+6: empty (advance notice) 
    RU->>RU: PA micro sleep for 6 slots 
    DL->>BUF: VoNR packet (5QI 1) 
    BUF->>SCH: Exempt: schedule immediately
```

## Load Guard Suspension

The guard parameter `batchingAllowedLoad` automatically suspends packet batching when cell PRB utilization exceeds the configured threshold, removing interaction with busy-hour capacity behavior.

# Cross-References

- [Parameters](parameters.md) - Defines configuration parameters referenced during feature operation, including `maxBatchDelay`, `targetSlotFill`, and `batchingAllowedLoad`.
- [Feature Overview](feature-overview.md) - High-level summary of the Energy-Optimized Slot Allocation feature.
