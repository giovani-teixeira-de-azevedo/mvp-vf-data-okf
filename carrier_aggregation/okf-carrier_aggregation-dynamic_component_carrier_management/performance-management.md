---
type: concept
resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf#performance-management
title: Performance Management
description: Monitoring methodology, KPIs, formulas, and counters for evaluating Dynamic
  Component Carrier Management performance.
tags:
- performance-management
- kpis
- counters
- metrics
- carrier-management
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:38:11+00:00'
  source_sha256: 52cc1921fc86fde3
sources:
- resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf
  title: Dynamic Component Carrier Management
---

The **Performance Management** framework for Dynamic Component Carrier Management defines the methodology, key performance indicators (KPIs), and counters required to monitor carrier load convergence, swap productivity, and signaling costs. Monitoring answers three fundamental questions: whether carrier load is converging (the balancing objective), whether swaps are productive (migrated UEs end up on better carriers), and whether the signaling cost is acceptable.

## Baselining and Monitoring Guidelines

Before activating the feature, a baseline should be established to enable pre- and post-activation comparison:
*   **Baselining Period:** Per-carrier PRB utilization spread and RRC reconfiguration counts should be baseline-monitored for one week prior to activation.
*   **Reporting Period:** The standard Recording Observation Period (ROP) is 15 minutes.
*   **Scope:** All performance counters are collected on a per-cell basis unless explicitly noted otherwise.

## Key Performance Indicators (KPIs)

The performance of the dynamic component carrier management feature is tracked via four key metrics:

*   **Load Spread:** This is the headline KPI. A healthy multi-carrier site converges to a spread below 15 percentage points within days.
*   **Migration Productivity:** This should exceed 85%. Lower values indicate that swapped UEs are not experiencing performance gains, typically because the `migrationMargin` parameter is too small relative to measurement noise.
*   **Migration Cap Hit Ratio:** A value near 100% for sustained periods indicates that the pacing cap is throttling convergence. In this scenario, the `maxMigrationsPerRop` parameter should be raised cautiously.
*   **Score-Order Setup Ratio:** Reflects the percentage of SCell setups that followed dynamic score order rather than static configuration.

### KPI Formulas

| KPI | Formula | Description |
| :--- | :--- | :--- |
| **Load Spread** | `ctrPrbLoadSpread` | Standard deviation of PRB load across managed carriers (pp). |
| **Migration Productivity** | $\frac{\text{ctrMigrationsImproved}}{\text{ctrMigrationsTotal}} \times 100$ | Share of swaps where post-swap UE throughput improved (%). |
| **Migration Cap Hit Ratio** | $\frac{\text{ctrMigrationCapHits}}{\text{ctrRopCount}} \times 100$ | Share of ROPs where the pacing cap was reached (%). |
| **Score-Order Setup Ratio** | $\frac{\text{ctrSetupScoreOrder}}{\text{ctrScellSetups}} \times 100$ | SCell setups that followed dynamic score order (%). |

*Note: In the formulas, the terms represent the counter values collected during the observation period.*

## Performance Counters

### Evaluation and Troubleshooting Notes

*   **Closed-Loop Quality Measure:** The counter `ctrMigrationsImproved` is evaluated exactly 10 seconds after each swap against the pre-swap throughput sample. This ensures that Migration Productivity measures genuine quality improvements rather than simple execution counts.
*   **Score Ties:** Operators should watch the `ctrScoreTies` counter. A high rate of ties indicates that the assigned weights yield indistinguishable scores, causing the feature to degenerate to static behavior. To resolve this, spread the weight values or verify that the load metrics are updating.

### Counter Definitions

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
| `ctrScellSetups` | SCell setup decisions taken | $0 \text{ to } 2^{31}$ | `int64` |
| `ctrSetupScoreOrder` | Setups where dynamic order differed from static order | $0 \text{ to } 2^{31}$ | `int64` |
| `ctrMigrationsTotal` | Rebalancing SCell swaps executed | $0 \text{ to } 2^{31}$ | `int64` |
| `ctrMigrationsImproved` | Swaps with improved post-swap throughput | $0 \text{ to } 2^{31}$ | `int64` |
| `ctrMigrationCapHits` | ROPs where `maxMigrationsPerRop` was reached | $0 \text{ to } 2^{31}$ | `int64` |
| `ctrScoreTies` | Setup decisions with tied top scores | $0 \text{ to } 2^{31}$ | `int64` |
| `ctrPrbLoadSpread` | Standard deviation of carrier PRB load per ROP (pp) | $0 \text{ to } 100$ | `int32` |
| `ctrRopCount` | ROPs elapsed (normalization counter) | $0 \text{ to } 2^{31}$ | `int64` |

# Cross-References

*   [Parameters](parameters.md) - Defines parameters such as `migrationMargin` and `maxMigrationsPerRop` mentioned in KPI and Cap Hit evaluations.
*   [Feature Operation](feature-operation.md) - Explains dynamic carrier selection and scoring mechanisms that influence score ties and setup orders.
