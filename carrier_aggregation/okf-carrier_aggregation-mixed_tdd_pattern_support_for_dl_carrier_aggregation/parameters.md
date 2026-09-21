---
type: reference-table
resource: data/vodafone-mvp/raw/Mixed TDD Pattern Support for DL Carrier Aggregation.pdf#parameters
title: Parameters
description: Configuration parameters for Mixed TDD Pattern Support for DL Carrier
  Aggregation, managed on NrFunction=1 with per-cell-relation overrides.
tags:
- TDD
- Carrier Aggregation
- Configuration
- Parameters
- NrFunction
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:38:54+00:00'
  source_sha256: 6d6f1334bbdc0622
sources:
- title: Mixed TDD Pattern Support for DL Carrier Aggregation
  resource: data/vodafone-mvp/raw/Mixed TDD Pattern Support for DL Carrier Aggregation.pdf
---

Configuration parameters for Mixed TDD Pattern Support for DL Carrier Aggregation are configured on `NrFunction=1`. 

These parameters include per-cell-relation overrides for pattern pairing rules. The default values are selected to enable conservative pairing, and the `pucchGroupMode` parameter serves as the primary capacity lever on loaded PCells.

## Parameter Reference Table

| Parameter | Description | Values | Datatype | Default |
| :--- | :--- | :--- | :--- | :--- |
| `mixedTddCaEnabled` | Enables mixed-pattern DL CA combinations | `true`, `false` | boolean | `false` |
| `maxPatternRatio` | Max periodicity ratio between aggregated CCs | 1–4 | int32 | 2 |
| `harqCodebookMode` | HARQ-ACK codebook for mixed combos | `TYPE1`, `TYPE2` | enum | `TYPE2` |
| `maxK1Extension` | Max additional k1 slots allowed to bridge patterns | 0–7 (slots) | int32 | 3 |
| `pucchGroupMode` | PUCCH group configuration | `SINGLE`, `DUAL` | enum | `SINGLE` |
| `halfDuplexPolicy` | Handling of half-duplex UEs | `EXCLUDE_CONFLICTS`, `NO_MIXED_CA` | enum | `EXCLUDE_CONFLICTS` |
| `pucchLoadGuard` | PCell PUCCH utilization above which new mixed combos are blocked | 50–95 (%) | int32 | 85 |

# Cross-References

* [Feature Operation](feature-operation.md) - Details on how these parameters affect the runtime operation of the mixed TDD pattern.
