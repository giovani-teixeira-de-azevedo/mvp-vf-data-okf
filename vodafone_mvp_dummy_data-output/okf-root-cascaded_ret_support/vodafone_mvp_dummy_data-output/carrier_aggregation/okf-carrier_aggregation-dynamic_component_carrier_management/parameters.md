---
type: reference-table
resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf#parameters
title: Parameters
description: Configuration parameters and weight rules for Dynamic Component Carrier
  Management on NrFunction=1.
tags:
- parameters
- nrfunction
- dynamic-component-carrier-management
- configuration
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T15:05:04+00:00'
  source_sha256: 872a1f140837c0be
sources:
- title: Dynamic Component Carrier Management
  resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf
---

Parameters for Dynamic Component Carrier Management live on `NrFunction=1`, with thresholds set overridable per cell. The three weights (`loadWeight`, `rsrpWeight`, `tputWeight`) must sum to 100. Defaults favor load, which suits capacity-driven deployments, while coverage-sensitive deployments (low-band anchor with weak mid-band) should raise `rsrpWeight`.

## Parameter Definitions

| Parameter | Description | Values | Datatype | Default |
| --- | --- | --- | --- | --- |
| `dynamicCcMgmtEnabled` | Enables dynamic carrier management | true, false | boolean | false |
| `loadWeight` | Weight of carrier load in the score | 0–100 | int32 | 50 |
| `rsrpWeight` | Weight of UE-measured RSRP in the score | 0–100 | int32 | 30 |
| `tputWeight` | Weight of achieved per-UE throughput in the score | 0–100 | int32 | 20 |
| `reevalPeriod` | Period between per-UE re-evaluations | 10–600 (s) | int32 | 60 |
| `rebalanceThr` | PRB load triggering rebalancing on a carrier | 50–100 (%) | int32 | 80 |
| `rebalanceHoldTimer` | Time above threshold before migration starts | 10–600 (s) | int32 | 60 |
| `migrationMargin` | Minimum score advantage for a swap | 1–50 (points) | int32 | 10 |
| `maxMigrationsPerRop` | Cap on SCell swaps per carrier per ROP | 0–1000 | int32 | 100 |

# Cross-References

- [Feature Operation](feature-operation.md)
- [Activation Procedure](activation-procedure.md)
