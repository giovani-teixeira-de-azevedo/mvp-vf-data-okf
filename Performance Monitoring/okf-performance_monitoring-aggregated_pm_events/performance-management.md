---
type: concept
resource: data/vodafone-mvp/raw/Aggregated PM Events.pdf#performance-management
title: Performance Management
description: Performance management KPIs and counters for monitoring event aggregation
  volume reduction and data integrity.
tags:
- performance-management
- kpi
- counters
- aggregation
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-02T14:06:14+00:00'
  source_sha256: d3e8ce25ebc27aed
sources:
- title: Aggregated PM Events
  resource: data/vodafone-mvp/raw/Aggregated PM Events.pdf
---

Performance management for the Aggregated PM Events feature monitors the volume reduction achieved and ensures data integrity is preserved without silent event loss or sustained overload sampling. All counters are collected per node over the standard 15-minute Result Output Period (ROP).

After feature activation, the node's PM file volume should be compared against a two-week pre-activation baseline for the same event job configuration. Downstream KPI time series computed from aggregated records must be verified to match those previously computed from raw events within rounding error.

## Key Performance Indicators (KPIs)

In a healthy node, the **Compression Ratio** typically sits between 5:1 and 20:1 depending on the aggregation dimension, and the **Sampled Record Ratio** stays at 0% outside extreme events. 

A rising **Sampled Record Ratio** indicates that the event rate exceeds the aggregation engine's capacity. To resolve this, reduce the number of aggregation groups, coarsen `aggregationDimension`, or lengthen `aggregationPeriod` (see [Parameters](parameters.md)).

A **Record Emission Success** below 100% indicates file-writer backpressure and should be correlated with O&M transport alarms.

| KPI | Formula | Description |
| :--- | :--- | :--- |
| **Compression Ratio** | `ctrRawEventsFolded / ctrAggRecordsEmitted` | Average raw events represented per aggregated record |
| **Sampled Record Ratio** | `(ctrAggRecordsSampled / ctrAggRecordsEmitted) * 100` | Share of records produced under overload sampling (%) |
| **Record Emission Success** | `(ctrAggRecordsEmitted / (ctrAggRecordsEmitted + ctrAggRecordsDropped)) * 100` | Share of aggregated records successfully written or streamed (%) |
| **Raw Bypass Ratio** | `(ctrRawEventsBypassed / (ctrRawEventsFolded + ctrRawEventsBypassed)) * 100` | Share of events emitted raw (trace, emergency, pass-through) (%) |

## Performance Counters

The counters below are the raw inputs to the KPIs. 

*   `ctrAggRecordsDropped` deserves special attention: it should be zero in steady state. Any sustained non-zero rate indicates that records are being lost between the aggregation engine and the file writer or streaming interface, resulting in unrecoverable data loss.
*   `ctrOverloadPeriods` counts aggregation periods during which sampling was active and serves as the earliest indicator of an under-dimensioned configuration.

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
| `ctrRawEventsFolded` | Raw events folded into aggregation accumulators | 0–2⁶³ | int64 |
| `ctrRawEventsBypassed` | Raw events emitted unaggregated (trace/emergency/pass-through) | 0–2⁶³ | int64 |
| `ctrAggRecordsEmitted` | Aggregated records written to file or streamed | 0–2³¹ | int64 |
| `ctrAggRecordsSampled` | Aggregated records produced with overload sampling active | 0–2³¹ | int64 |
| `ctrAggRecordsDropped` | Aggregated records lost due to writer backpressure | 0–2³¹ | int64 |
| `ctrOverloadPeriods` | Aggregation periods executed in overload-sampling state | 0–2³¹ | int64 |

# Cross-References

*   [Parameters](parameters.md) — Configuration parameters such as `aggregationDimension` and `aggregationPeriod`
*   [Activation Procedure](activation-procedure.md) — Steps for activating the feature and establishing the baseline
*   [Feature Operation](feature-operation.md) — Details on the aggregation engine and overload handling
