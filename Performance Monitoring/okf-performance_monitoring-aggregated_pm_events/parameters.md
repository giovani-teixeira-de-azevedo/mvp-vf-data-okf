---
type: reference-table
resource: data/vodafone-mvp/raw/Aggregated PM Events.pdf#parameters
title: Parameters
description: Configuration parameters controlling PM event aggregation behavior under
  NodeRoot=1,NrFunction=1,PmEventAggregation=<group>.
tags:
- parameters
- configuration
- pm-events
- aggregation
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-02T14:06:08+00:00'
  source_sha256: 1b649e9953ed4ef2
sources:
- resource: data/vodafone-mvp/raw/Aggregated PM Events.pdf
  title: Aggregated PM Events
---

This section details the configuration parameters that control the aggregation behavior of Performance Management (PM) events. These parameters are configured per aggregation group under the following path in the configuration hierarchy:

`NodeRoot=1,NrFunction=1,PmEventAggregation=<group>`

The default settings are optimized to produce per-cell aggregation at 60-second intervals, which is suitable for most analytics pipelines. Shortening the `aggregationPeriod` should only be done when near-real-time dashboards require it, as shorter periods significantly increase record volume.

## Parameter Reference

| Parameter | Description | Values | Datatype | Default |
| :--- | :--- | :--- | :--- | :--- |
| `aggregationState` | Enables the aggregation group | `DISABLED`, `ENABLED` | enum | `DISABLED` |
| `aggregationPeriod` | Period over which events are folded into one record | 10–900 (s) | int32 | 60 |
| `aggregationDimension` | Dimension key for accumulator instances | `CELL`, `CELL_5QI`, `CELL_BEAM`, `CELL_SLICE` | enum | `CELL` |
| `eventTypeList` | PM event types included in the group | list of event type IDs | string | empty |
| `histogramBinEdges` | Bin edges for the value histogram | 2–16 comma-separated values | string | empty |
| `rawPassThrough` | Also emit raw events for this group | `true`, `false` | boolean | `false` |
| `overloadSamplingRatio` | Minimum sampling ratio applied under CPU overload | 1–100 (%) | int32 | 10 |
| `emergencyRawAlways` | Always emit emergency-procedure events raw | `true`, `false` | boolean | `true` |

# Cross-References

* [Feature Operation](feature-operation.md) — Details on how these parameters affect the runtime operation of the aggregation feature.
* [Performance Management](performance-management.md) — Information on performance management and event handling.
