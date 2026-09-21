---
type: concept
resource: data/vodafone-mvp/raw/Mixed Bandwidth Support for Carrier Aggregation High-Band.pdf#performance-management
title: Performance Management
description: Outlines performance monitoring goals, KPIs, and PM counters for Mixed
  Bandwidth Support for Carrier Aggregation.
tags:
- performance-management
- kpi
- counters
- carrier-aggregation
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:38:36+00:00'
  source_sha256: 6595eb7d6fe873ec
sources:
- title: Mixed Bandwidth Support for Carrier Aggregation High-
  resource: data/vodafone-mvp/raw/Mixed Bandwidth Support for Carrier Aggregation
    High-Band.pdf
---

This section defines the performance management framework for Mixed Bandwidth Support for Carrier Aggregation High-Band. It outlines the monitoring goals, key performance indicators (KPIs), and PM counters required to analyze the utilization and efficiency of mixed-bandwidth carrier combinations.

## Monitoring Goals

The performance monitoring framework focuses on two primary areas:
1. **Utilization**: Confirming that odd-sized carriers are successfully absorbed into Carrier Aggregation (CA) service.
2. **Efficiency**: Verification that mixed bandwidth combinations perform as well per MHz as uniform bandwidth combinations.

To assess performance, baseline per-carrier Physical Resource Block (PRB) utilization and User Equipment (UE) throughput must be captured before activation. After activation, these are compared against per-MHz-normalized values. Counters accumulate per cell per standard 15-minute Reporting Period (ROP).

## Key Performance Indicators (KPIs)

* **Mixed Combo Ratio**: Reflects feature adoption. On an irregular spectrum holding with a healthy CA-capable UE base, this ratio typically reaches 20% to 50% of CA configurations within days of activation.
* **Small CC Utilization**: Represents the payback KPI. The previously stranded carrier's utilization should rise to meet the utilization of its larger siblings. If utilization remains low despite a healthy Mixed Combo Ratio, verify whether the `bwNormalizedPf` parameter is enabled, as an un-normalized Proportional Fair (PF) metric can starve smaller Component Carriers (CCs).
* **Per-MHz Efficiency Delta**: Expected to remain within $\pm5\%$. A larger negative delta indicates capability-parsing fallbacks may be downgrading combinations.
* **Combo Downgrade Rate**: Identifies CA setups falling back to a smaller combination.

### KPI Formula Table

| KPI | Formula | Description |
| :--- | :--- | :--- |
| **Mixed Combo Ratio** | `(ctrMixedBwConfigs / ctrCaConfigsTotal) * 100` | Share of CA configurations using mixed bandwidths (%) |
| **Small CC Utilization** | `(ctrSmallCcPrbUsed / ctrSmallCcPrbAvail) * 100` | PRB utilization of sub-100 MHz CCs (%) |
| **Per-MHz Efficiency Delta** | `((ctrMixedComboTputPerMhz - ctrUniformComboTputPerMhz) / ctrUniformComboTputPerMhz) * 100` | Throughput per MHz comparison, mixed vs uniform combos (%) |
| **Combo Downgrade Rate** | `(ctrComboDowngrades / ctrCaConfigsTotal) * 100` | CA setups falling back to a smaller combination (%) |

## Performance Counters

The counter `ctrComboDowngrades` is highly critical for identifying missing performance gains. It increments whenever the scheduler/builder is forced to discard the largest valid combination due to:
* UE fallback-group restrictions, or
* The `maxDistinctBwPerUe` capability cap.

A high downgrade rate concentrated on specific UE models indicates a capability-signaling issue to address with the device ecosystem rather than a network fault.

### Counter Registry

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
| `ctrCaConfigsTotal` | CA configurations built | 0–2³¹ | int64 |
| `ctrMixedBwConfigs` | Configurations with heterogeneous CC bandwidths | 0–2³¹ | int64 |
| `ctrComboDowngrades` | Largest combination discarded due to restrictions | 0–2³¹ | int64 |
| `ctrSmallCcPrbUsed` | PRBs scheduled on sub-100 MHz CCs | 0–2³¹ | int64 |
| `ctrSmallCcPrbAvail` | PRBs available on sub-100 MHz CCs | 0–2³¹ | int64 |
| `ctrMixedComboTputPerMhz` | Avg UE throughput per MHz in mixed combos (kbps/MHz) | 0–2³¹ | int64 |
| `ctrUniformComboTputPerMhz` | Avg UE throughput per MHz in uniform combos (kbps/MHz) | 0–2³¹ | int64 |

# Cross-References

* [Parameters](parameters.md) - For the configuration of `bwNormalizedPf` and `maxDistinctBwPerUe`.
