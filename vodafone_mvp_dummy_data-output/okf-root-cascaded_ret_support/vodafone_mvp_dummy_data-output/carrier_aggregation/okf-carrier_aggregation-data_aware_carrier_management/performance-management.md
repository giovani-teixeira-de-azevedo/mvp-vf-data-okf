---
type: concept
resource: data/vodafone-mvp/raw/Data-Aware Carrier Management.pdf#performance-management
title: Performance Management
description: Guidance, Key Performance Indicators (KPIs), and counters for verifying
  the efficiency and safety of Data-Aware Carrier Management.
tags:
- performance-management
- kpis
- counters
- scell
- carrier-aggregation
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T13:48:24+00:00'
  source_sha256: 218c62c4ee9b3d56
sources:
- resource: data/vodafone-mvp/raw/Data-Aware Carrier Management.pdf
  title: Data-Aware Carrier Management
---

Performance management verifies two properties of the Data-Aware Carrier Management feature: efficiency (avoiding unnecessary Secondary Cell (SCell) configurations and activations) and safety (ensuring genuine high-demand bursts are promoted fast enough to protect throughput).

A pre-activation baseline of SCell activation counts, RRC reconfiguration volume, and downlink burst throughput percentiles should be established over at least one week, and then compared with the same hours post-activation. All counters accumulate per cell over the 15-minute Result Output Period (ROP).

## Key Performance Indicators (KPIs)

A healthy deployment shows Promotion Latency below 60 ms at the 95th percentile so that bursts do not wait noticeably for aggregation. A Suppression Ratio between 30% and 60% is typical for smartphone-dominated traffic:
- A value near zero indicates thresholds are set so low that everything is classified BULK (yielding no benefit).
- A value above 80% suggests that `bulkBurstThr` is set too high and large bursts are being served on the Primary Cell (PCell) only, which manifests as degraded high-percentile throughput.

| KPI | Formula | Description |
| --- | --- | --- |
| Suppression Ratio | `ctrScellConfigSuppressed / (ctrScellConfigSuppressed + ctrScellConfigured) * 100` | Share of candidate SCell configurations avoided (%) |
| Promotion Latency P95 | `ctrPromotionDelayP95` | 95th percentile time from burst arrival to SCell activation (ms) |
| Promotion Accuracy | `ctrBulkPromotions / (ctrBulkPromotions + ctrLatePromotions) * 100` | Share of BULK bursts promoted before 20% of the burst was served (%) |
| Class Stability | `ctrClassChanges / ctrConnectedUeSamples` | Class changes per UE sample; high values indicate threshold ping-pong |

## Performance Counters

The counters capture the estimator's decisions and their timeliness. `ctrLatePromotions` is the safety counter to watch: it increments when a burst that was ultimately classified BULK had already delivered more than 20% of its volume before SCell activation completed, indicating that the user paid a throughput cost. A rising trend calls for lowering `bulkBurstThr` or shortening `classDwellTimer`.

| Counter | Description | Range | Datatype |
| --- | --- | --- | --- |
| `ctrScellConfigured` | SCell configuration events executed | 0–2³¹ | int64 |
| `ctrScellConfigSuppressed` | SCell configurations avoided by demand classification | 0–2³¹ | int64 |
| `ctrBulkPromotions` | Timely promotions to BULK | 0–2³¹ | int64 |
| `ctrLatePromotions` | Promotions completing after 20% of burst served | 0–2³¹ | int64 |
| `ctrClassChanges` | Demand class transitions | 0–2³¹ | int64 |
| `ctrConnectedUeSamples` | Per-UE estimator samples | 0–2³¹ | int64 |
| `ctrPromotionDelayP95` | 95th percentile promotion delay per ROP (ms) | 0–10000 | int32 |

# Cross-References

- [Parameters](parameters.md)
