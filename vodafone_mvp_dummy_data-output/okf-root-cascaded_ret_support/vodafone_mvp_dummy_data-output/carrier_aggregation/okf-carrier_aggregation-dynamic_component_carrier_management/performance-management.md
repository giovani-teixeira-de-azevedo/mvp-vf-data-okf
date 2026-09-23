---
type: concept
resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf#performance-management
title: PERFORMANCE MANAGEMENT
description: Defines performance monitoring guidelines, KPIs, and counters for Dynamic
  Component Carrier Management.
tags:
- performance-management
- kpis
- counters
- carrier-load
- rebalancing
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T15:06:09+00:00'
  source_sha256: 52cc1921fc86fde3
sources:
- title: Dynamic Component Carrier Management
  resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf
---

This section details performance management guidelines, Key Performance Indicators (KPIs), and performance counters for Dynamic Component Carrier Management.

Monitoring answers three questions: is carrier load actually converging (the balance objective), are swaps productive (migrated UEs end up on better carriers), and is the signaling cost acceptable. Baseline the per-carrier PRB utilization spread and RRC reconfiguration counts for one week before activation; the ROP is the standard 15 minutes and all counters are per cell unless noted.

## KPIs

Load Spread is the headline KPI: a healthy multi-carrier site converges to a spread below 15 percentage points within days. Migration Productivity should exceed 85% — lower values mean swapped UEs are not gaining, typically because `migrationMargin` is too small relative to measurement noise. Migration Cap Hit Ratio near 100% for sustained periods means the pacing cap is throttling convergence; raise `maxMigrationsPerRop` cautiously.

| KPI | Formula | Description |
| --- | --- | --- |
| Load Spread | `ctrPrbLoadSpread` | Std deviation of PRB load across managed carriers (pp) |
| Migration Productivity | `ctrMigrationsImproved` / `ctrMigrationsTotal` × 100 | Share of swaps where post-swap UE throughput improved (%) |
| Migration Cap Hit Ratio | `ctrMigrationCapHits` / `ctrRopCount` × 100 | Share of ROPs where the pacing cap was reached (%) |
| Score-Order Setup Ratio | `ctrSetupScoreOrder` / `ctrScellSetups` × 100 | SCell setups that followed dynamic score order (%) |

## Counters

`ctrMigrationsImproved` is evaluated 10 s after each swap against the pre-swap throughput sample, making Migration Productivity a genuine closed-loop quality measure rather than an activity count. Watch `ctrScoreTies`: a high tie rate means the weights produce indistinguishable scores and the feature degenerates to static behavior — spread the weights or verify load metrics are updating.

| Counter | Description | Range | Datatype |
| --- | --- | --- | --- |
| `ctrScellSetups` | SCell setup decisions taken | 0–2³¹ | int64 |
| `ctrSetupScoreOrder` | Setups where dynamic order differed from static order | 0–2³¹ | int64 |
| `ctrMigrationsTotal` | Rebalancing SCell swaps executed | 0–2³¹ | int64 |
| `ctrMigrationsImproved` | Swaps with improved post-swap throughput | 0–2³¹ | int64 |
| `ctrMigrationCapHits` | ROPs where `maxMigrationsPerRop` was reached | 0–2³¹ | int64 |
| `ctrScoreTies` | Setup decisions with tied top scores | 0–2³¹ | int64 |
| `ctrPrbLoadSpread` | Std deviation of carrier PRB load per ROP (pp) | 0–100 | int32 |
| `ctrRopCount` | ROPs elapsed (normalization counter) | 0–2³¹ | int64 |

# Cross-References

- [PARAMETERS](parameters.md) — Defines `migrationMargin` and `maxMigrationsPerRop` referenced in KPI analysis.
