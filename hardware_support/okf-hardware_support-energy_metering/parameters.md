---
type: reference-table
resource: data/vodafone-mvp/raw/Energy Metering.pdf#parameters
title: PARAMETERS
description: Configuration parameters for the Energy Metering feature, controlling
  polling cadence, estimation fallback, and alarm thresholds.
tags:
- energy-metering
- configuration
- parameters
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:43:18+00:00'
  source_sha256: eece738063db2505
sources:
- title: Energy Metering
  resource: data/vodafone-mvp/raw/Energy Metering.pdf
---

This section describes the configuration parameters for the Energy Metering feature. The feature is largely plug-and-measure with minimal configuration, primarily controlling polling cadence, estimation fallback, and optional high-consumption alarms.

The alarm threshold (`highConsumptionThr`) is particularly recommended for tuning based on the site class, as normal power draw varies significantly between site types (for example, a small indoor site compared to a 64T64R macro site).

| Parameter | Description | Values | Datatype | Default |
| :--- | :--- | :--- | :--- | :--- |
| `meteringEnabled` | Enables energy accumulation per unit | `true`, `false` | boolean | `true` |
| `samplingInterval` | Internal poll interval of unit meters | 1–60 (s) | int32 | 1 |
| `estimationFallback` | Use power-model estimates for unmetered units | `true`, `false` | boolean | `true` |
| `siteAggregation` | Include site power system delta in node total | `true`, `false` | boolean | `true` |
| `highConsumptionThr` | Node-level alarm threshold for average power | 100–20000 (W) | int32 | 10000 |
| `highConsumptionTimer` | Time above threshold before alarm | 60–3600 (s) | int32 | 900 |
| `reportEstimatedSeparately` | Split measured vs estimated energy in counters | `true`, `false` | boolean | `true` |

# Cross-References

- [Feature Overview](feature-overview.md)
- [Feature Operation](feature-operation.md)
- [Performance Management](performance-management.md)
- [Activation Procedure](activation-procedure.md)
