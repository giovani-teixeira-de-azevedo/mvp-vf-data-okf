---
type: concept
resource: data/vodafone-mvp/raw/Aggregated PM Events.pdf#feature-depedencies
title: Feature Dependencies
description: Outlines the feature, hardware, and network dependencies, as well as
  operational limitations for the Aggregated PM Events feature.
tags:
- dependencies
- limitations
- licensing
- hardware
- network
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-02T14:06:14+00:00'
  source_sha256: 95c5d854c01c2167
sources:
- title: Aggregated PM Events
  resource: data/vodafone-mvp/raw/Aggregated PM Events.pdf
---

This section outlines the feature, hardware, and network dependencies, as well as the operational limitations for the Aggregated PM Events feature. It describes the licensing, hardware constraints, network schema requirements, and functional boundaries that must be verified prior to rollout.

The feature changes only how PM event data is packaged, but the choice of aggregation dimensions interacts with other observability features, and downstream mediation systems must understand the aggregated record format.

## Feature Dependencies

* **Licensing and Activation:** Requires a valid license key (`FAK-30110`) installed and `FeatureCtrl=AggregatedPmEvents` set to `ACTIVATED` under `NrFunction=1`.
* **Streaming of PM Events:** Interworks with Streaming of PM Events. Aggregated records can be streamed instead of, or in addition to, being written to ROP files.
* **EBS Counters Enabler:** Interworks with EBS Counters Enabler. Event-based statistics jobs can consume aggregated records directly, reducing EBS processing load.
* **Slicing Observability:** Per-slice aggregation dimensions require NR Enhanced Slicing Observability to be activated.
* **UE Trace:** Does not affect NR UE Trace; trace sessions always receive raw, unaggregated events.

## Hardware Dependencies

* **Hardware Requirements:** No dedicated hardware is required. Aggregation runs on the baseband unit's O&M processor complex.
* **Capacity Constraints:** Nodes with baseband generation B1 hardware support a maximum of 32 concurrent aggregation groups instead of 64.

## Network Dependencies

* **Mediation/Analytics Support:** The northbound mediation/analytics system must support the aggregated event record schema (record format version 3 or later of the PM event file specification).
* **ROP File Collection:** If ROP file collection uses SFTP pull, no change is needed; file naming is unchanged.

## Limitations

* **Aggregation Period:** Aggregation periods shorter than 10 seconds are not supported.
* **Emergency Calls:** Events belonging to emergency-call procedures are always emitted raw in addition to being aggregated, to preserve regulatory traceability.
* **Histogram Bin Modification:** Histogram bin edges can only be changed when the affected aggregation group is disabled; on-the-fly modification is rejected.
* **Aggregation Group Limits:** Maximum 64 aggregation groups per node; each group supports at most 16 histogram bins.

# Cross-References

* [Feature Overview](feature-overview.md)
* [Parameters](parameters.md)
* [Activation Procedure](activation-procedure.md)
