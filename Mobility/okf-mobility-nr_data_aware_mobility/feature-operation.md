---
type: concept
resource: data/vodafone-mvp/raw/NR Data-Aware Mobility.pdf#feature-operation
title: Feature Operation
description: Describes the operational mechanisms of NR Data-Aware Mobility, including
  activity classification, handover deferral, gap-seeking, and safety enforcement.
tags:
- mobility
- handover
- gap-seeker
- activity-classifier
- RRC-connected
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T10:02:21+00:00'
  source_sha256: b1e428401d67c577
sources:
- resource: data/vodafone-mvp/raw/NR Data-Aware Mobility.pdf
  title: NR Data-Aware Mobility
---

This section describes the operational mechanisms of the NR Data-Aware Mobility feature. It details how the system classifies UE activity, defers handovers for high-activity UEs, seeks transmission gaps, and enforces safety limits to prevent radio link failures.

## Activity Classification

The activity classifier evaluates each RRC-connected UE over a sliding 200 ms window. A UE is classified as **HIGH-activity** when:
* Its aggregated downlink and uplink (DL+UL) RLC buffer occupancy exceeds `activityBufferThr`, or
* Its scheduled throughput exceeds `activityRateThr`.

To prevent rapid switching between activity states (flapping), class transitions are hysteresis-protected.

## Handover Deferral and Gap-Seeking

When a measurement report for a deferrable event is received for a HIGH-activity UE, the mobility engine defers the handover by re-arming the event with modified parameters:
* **Offset Adjustment**: `deferOffset` is added to the A3 offset.
* **Time-to-Trigger (TTT) Extension**: `deferTtt` is appended to the time-to-trigger.

If the handover condition persists after this deferral, the handover becomes due, and the **gap-seeker** mechanism takes over:
1. The scheduler flags the UE.
2. The system monitors for an inter-packet gap where both RLC directions (DL and UL) fall below `bufferGapThr`.
3. The first such gap triggers immediate handover preparation toward the best-scored target cell.
4. If no suitable gap is found within the `gapSearchWindow`, the handover executes anyway. Deferral is designed to shape the timing of mobility to minimize packet loss, but it never blocks mobility indefinitely.

## Safety Enforcement

Safety is continuously monitored and enforced during the deferral period. If the serving-cell RSRP falls below `criticalRsrpFloor` at any point during deferral, the pending handover is immediately escalated to execution with the strongest target cell, bypassing the normal scoring process.

# Cross-References

* [Feature Overview](feature-overview.md) — Overview of the NR Data-Aware Mobility feature.
* [Parameters](parameters.md) — Detailed definitions of the configuration parameters such as `activityBufferThr`, `activityRateThr`, `deferOffset`, `deferTtt`, `bufferGapThr`, `gapSearchWindow`, and `criticalRsrpFloor`.
