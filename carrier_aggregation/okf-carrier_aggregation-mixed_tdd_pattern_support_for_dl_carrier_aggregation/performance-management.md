---
type: concept
resource: data/vodafone-mvp/raw/Mixed TDD Pattern Support for DL Carrier Aggregation.pdf#performance-management
title: Performance Management
description: Monitoring and performance evaluation metrics, KPIs, and counters for
  Mixed TDD Pattern Support for DL Carrier Aggregation.
tags:
- performance-management
- kpi
- counters
- tdd-patterns
- carrier-aggregation
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:38:58+00:00'
  source_sha256: 2a6c9371f36ad7fe
sources:
- resource: data/vodafone-mvp/raw/Mixed TDD Pattern Support for DL Carrier Aggregation.pdf
  title: Mixed TDD Pattern Support for DL Carrier Aggregation
---

This section outlines the performance management framework, including KPIs and counters, used to monitor and evaluate the performance of Mixed TDD Pattern Support for DL Carrier Aggregation. Monitoring focuses on carrier aggregation adoption, feedback latency overhead, and PCell PUCCH load handling.

Before activating the feature, baseline PCell PUCCH utilization and per-carrier CA throughput should be monitored for a week. All performance counters accumulate per cell per 15-minute Reporting Object Period (ROP).

---

## Key Performance Indicators (KPIs)

The following KPIs are used to assess the adoption, efficiency, and overhead of mixed TDD carrier aggregation:

| KPI | Formula | Description |
| :--- | :--- | :--- |
| **Mixed CA Adoption** | `(ctrMixedCaConfigs / ctrCaConfigsTotal) * 100` | Share of CA configurations spanning mixed TDD patterns (%). Typically reaches 20–60% by traffic volume. |
| **HARQ RTT Delta** | `ctrHarqRttMixedAvg - ctrHarqRttUniformAvg` | Average HARQ Round Trip Time (RTT) difference between mixed and uniform SCells (in slots). Expected delta is 1–3 slots; larger values suggest k1 sets are being stretched due to PUCCH congestion. |
| **PUCCH Guard Block Rate** | `(ctrPucchGuardBlocks / ctrMixedCaAttempts) * 100` | Share of mixed TDD CA combinations blocked by the PUCCH load guard (%). This should remain near zero; sustained blocking indicates the PCell PUCCH is a bottleneck. |
| **Mixed SCell Efficiency** | `(ctrMixedScellTput / ctrUniformScellTput) * 100` | Mixed SCell scheduled throughput relative to uniform SCells (%). |

*Note: If sustained blocking is observed on the PUCCH Guard Block Rate, it is recommended to switch `pucchGroupMode` to `DUAL` before expanding the rollout. See [Parameters](parameters.md) for parameter details.*

---

## Performance Counters

The following counters accumulate per cell per 15-minute ROP:

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
| `ctrCaConfigsTotal` | Total Carrier Aggregation (CA) configurations built. | 0–2³¹ | int64 |
| `ctrMixedCaConfigs` | Configurations spanning different (mixed) TDD patterns. | 0–2³¹ | int64 |
| `ctrMixedCaAttempts` | Mixed TDD CA combinations attempted (including blocked attempts). | 0–2³¹ | int64 |
| `ctrPucchGuardBlocks` | Mixed TDD CA combinations blocked by the PUCCH load guard (`pucchLoadGuard`). | 0–2³¹ | int64 |
| `ctrHarqRttMixedAvg` | Average HARQ RTT on mixed SCells per ROP (measured in slots × 100). | 0–2³¹ | int64 |
| `ctrHarqRttUniformAvg` | Average HARQ RTT on uniform SCells per ROP (measured in slots × 100). | 0–2³¹ | int64 |
| `ctrMixedScellTput` | Average scheduled throughput on mixed SCells (kbps). | 0–2³¹ | int64 |
| `ctrUniformScellTput` | Average scheduled throughput on uniform SCells (kbps). | 0–2³¹ | int64 |
| `ctrCodebookMismatch` | HARQ codebook size mismatches detected. | 0–2³¹ | int64 |

### HARQ Codebook Mismatch Analysis
The counter `ctrCodebookMismatch` increments when the received HARQ-ACK codebook size does not match the expected dynamic codebook. This discrepancy points to UE implementation issues with Type-2 codebooks across differing TDD patterns. If a non-negligible rate of mismatches is concentrated on specific device models, it justifies excluding those specific models via the network's UE capability handling policy.

# Cross-References

* [Parameters](parameters.md) — Configuration parameters including `pucchGroupMode` and `pucchLoadGuard` referenced in performance monitoring.
