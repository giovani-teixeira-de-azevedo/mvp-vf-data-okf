---
type: concept
resource: data/vodafone-mvp/raw/NR Data-Aware Mobility.pdf#feature-operation
title: FEATURE OPERATION
description: Describes the operational mechanisms of NR Data-Aware Mobility, including
  activity classification, handover deferral, gap-seeking, and safety fallback.
tags:
- NR Data-Aware Mobility
- Handover Deferral
- Activity Classifier
- Gap-Seeker
- Mobility Engine
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T09:06:47+00:00'
  source_sha256: b1e428401d67c577
sources:
- title: NR Data-Aware Mobility
  resource: data/vodafone-mvp/raw/NR Data-Aware Mobility.pdf
---

This section describes the operational mechanisms of the NR Data-Aware Mobility feature, detailing how the activity classifier, mobility engine, gap-seeker, and safety fallback mechanisms interact to manage handovers for active User Equipments (UEs).

## Operational Mechanisms

The feature operates through a sequence of evaluation, deferral, gap-seeking, and safety enforcement stages.

### 1. Activity Classification
The activity classifier evaluates each RRC-connected UE over a sliding **200 ms window**. A UE is classified as **HIGH-activity** when:
* Its aggregated Downlink (DL) + Uplink (UL) RLC buffer occupancy exceeds `activityBufferThr`, or
* Its scheduled throughput exceeds `activityRateThr`.

To prevent rapid switching between states, class transitions are hysteresis-protected to avoid flapping.

### 2. Handover Deferral
When a measurement report for a deferrable event arrives for a UE classified as HIGH-activity, the mobility engine delays the handover by re-arming the event with modified parameters:
* `deferOffset` is added to the A3 offset.
* `deferTtt` is appended to the time-to-trigger (TTT).

### 3. Gap-Seeking and Execution
If the handover condition persists after the deferral period, the handover becomes due, and the gap-seeker mechanism takes over:
1. The scheduler flags the UE.
2. The first inter-packet gap where both RLC directions fall below `bufferGapThr` triggers immediate handover preparation toward the best-scored target.
3. If no suitable gap is found within the `gapSearchWindow`, the handover executes anyway. Deferral is designed to shape the timing of mobility, but it never blocks it.

### 4. Safety Fallback
Safety is continuously enforced throughout the deferral process. If the serving-cell Reference Signal Received Power (RSRP) falls below `criticalRsrpFloor` at any point during deferral, the pending handover escalates immediately to execution with the strongest target, bypassing the scoring process.

# Cross-References

* [Feature Overview](feature-overview.md) — General overview of the NR Data-Aware Mobility feature.
* [Parameters](parameters.md) — Detailed definitions of the parameters used in this operation (e.g., `activityBufferThr`, `activityRateThr`, `deferOffset`, `deferTtt`, `bufferGapThr`, `gapSearchWindow`, and `criticalRsrpFloor`).
