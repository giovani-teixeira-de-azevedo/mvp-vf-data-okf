---
type: reference-table
resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf#parameters
title: Parameters
description: Configuration parameters for Dynamic Component Carrier Management living
  on NrFunction=1.
tags:
- parameters
- configuration
- dynamic-cc-management
- nr-function
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:38:03+00:00'
  source_sha256: 872a1f140837c0be
sources:
- resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf
  title: Dynamic Component Carrier Management
---

This section outlines the configuration parameters for the Dynamic Component Carrier Management feature. These parameters reside on `NrFunction=1`, with thresholds that can be overridden at the cell level.

## Scoring Weight Considerations

The scoring mechanism utilizes three weight parameters (`loadWeight`, `rsrpWeight`, and `tputWeight`) which **must sum to 100**. 

- **Capacity-Driven Deployments**: The default parameter values favor load weighting (`loadWeight = 50`), which is suitable for capacity-driven scenarios.
- **Coverage-Sensitive Deployments**: For deployments that are coverage-sensitive (such as a low-band anchor with a weak mid-band carrier), it is recommended to increase `rsrpWeight` to prioritize signal strength in the scoring calculation.

## Configuration Parameters Reference

| Parameter | Description | Values | Datatype | Default |
| :--- | :--- | :--- | :--- | :--- |
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

- [Feature Overview](feature-overview.md) - Overview of the Dynamic Component Carrier Management feature.
- [Feature Operation](feature-operation.md) - Detail on how these parameters are used in the decision-making and scoring algorithms.
