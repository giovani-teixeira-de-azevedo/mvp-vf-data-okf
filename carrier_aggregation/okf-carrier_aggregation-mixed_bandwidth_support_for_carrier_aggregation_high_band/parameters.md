---
type: reference-table
resource: data/vodafone-mvp/raw/Mixed Bandwidth Support for Carrier Aggregation High-Band.pdf#parameters
title: Parameters
description: Configuration parameters and policy settings for Mixed Bandwidth Support
  for Carrier Aggregation High-Band.
tags:
- carrier-aggregation
- mixed-bandwidth
- parameters
- configuration
- high-band
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:38:34+00:00'
  source_sha256: aaece81f4ed770cf
sources:
- title: Mixed Bandwidth Support for Carrier Aggregation High-
  resource: data/vodafone-mvp/raw/Mixed Bandwidth Support for Carrier Aggregation
    High-Band.pdf
---

This section details the configuration parameters for Mixed Bandwidth Support for Carrier Aggregation High-Band. These parameters are configured under `NrFunction=1`, with the allowed bandwidth set per sector carrier defined by the carrier configuration itself. 

The primary policy decision governed by these settings is whether small component carriers (CCs) are eligible as Primary Cell (PCell) targets or restricted to Secondary Cell (SCell) duty.

## Parameter Reference

| Parameter | Description | Values | Datatype | Default |
| :--- | :--- | :--- | :--- | :--- |
| `mixedBwCaEnabled` | Enables mixed bandwidth CA combinations | true, false | boolean | false |
| `minScellBandwidth` | Smallest CC bandwidth eligible as SCell | 50, 100, 200 (MHz) | enum | 50 |
| `smallCcPcellAllowed` | Allow CCs below 100 MHz as PCell | true, false | boolean | false |
| `maxAggregatedBw` | Cap on aggregated bandwidth per UE | 100–800 (MHz) | int32 | 800 |
| `bwNormalizedPf` | Normalize PF metric by CC bandwidth | true, false | boolean | true |
| `refarmReconfigMode` | UE reconfiguration pacing after carrier resize | LAZY, PACED, IMMEDIATE | enum | LAZY |
| `maxDistinctBwPerUe` | Max distinct CC bandwidths in one UE config | 2–4 | int32 | 3 |

# Cross-References

* [Feature Operation](feature-operation.md) — For details on how these parameters govern scheduling and carrier combination behavior.
* [Activation Procedure](activation-procedure.md) — For instructions on enabling this feature using these parameters.
* [Deactivation Procedure](deactivation-procedure.md) — For instructions on disabling this feature.
