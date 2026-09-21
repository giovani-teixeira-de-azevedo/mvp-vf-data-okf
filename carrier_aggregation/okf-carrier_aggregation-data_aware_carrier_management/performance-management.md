---
type: concept
resource: data/vodafone-mvp/raw/Data-Aware Carrier Management.pdf#performance-management
title: Performance Management
description: Performance management guidelines, KPIs, and PM counters for validating
  the efficiency and safety of the Data-Aware Carrier Management feature.
tags:
- KPIs
- PM Counters
- SCell Configuration
- Performance Management
- Data-Aware Carrier Management
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:37:42+00:00'
  source_sha256: 218c62c4ee9b3d56
sources:
- resource: data/vodafone-mvp/raw/Data-Aware Carrier Management.pdf
  title: Data-Aware Carrier Management
---

This section covers the performance management guidelines, Key Performance Indicators (KPIs), and Performance Measurement (PM) counters for verifying the efficiency and safety of the Data-Aware Carrier Management feature. These metrics help establish a pre-activation baseline and assess feature performance over 15-minute Reporting Observed Periods (ROPs).

## Overview

Performance management for Data-Aware Carrier Management verifies two properties:
*   **Efficiency**: Ensuring that unnecessary secondary cell (SCell) configurations and activations are avoided.
*   **Safety**: Ensuring that genuine high-demand bursts are still promoted fast enough to protect downlink throughput.

To monitor performance, establish a pre-activation baseline of SCell activation counts, RRC reconfiguration volume, and downlink burst throughput percentiles over at least one week, then compare the same hours post-activation. All counters accumulate per cell over the 15-minute ROP.

## Key Performance Indicators (KPIs)

A healthy deployment exhibits the following KPI characteristics:
*   **Promotion Latency P95**: Should be below 60 ms at the 95th percentile. Bursts should not wait noticeably for carrier aggregation.
*   **Suppression Ratio**: Typically between 30% and 60% for smartphone-dominated traffic. 
    *   A value near zero indicates that thresholds are set so low that everything is classified as `BULK` (resulting in no benefit).
    *   A value above 80% suggests that the `bulkBurstThr` parameter is set too high, causing large bursts to be served on the PCell only, which degrades high-percentile throughput.

| KPI | Formula | Description |
| :--- | :--- | :--- |
| **Suppression Ratio** | $$\frac{\text{ctrScellConfigSuppressed}}{\text{ctrScellConfigSuppressed} + \text{ctrScellConfigured}} \times 100$$ | Share of candidate SCell configurations avoided (%) |
| **Promotion Latency P95** | `ctrPromotionDelayP95` | 95th percentile time from burst arrival to SCell activation (ms) |
| **Promotion Accuracy** | $$\frac{\text{ctrBulkPromotions}}{\text{ctrBulkPromotions} + \text{ctrLatePromotions}} \times 100$$ | Share of `BULK` bursts promoted before 20% of the burst was served (%) |
| **Class Stability** | `ctrClassChanges / ctrConnectedUeSamples` | Class changes per UE sample; high values indicate threshold ping-pong |

*Note: Formulas can also be represented in text form as:*
*   **Suppression Ratio**: `ctrScellConfigSuppressed / (ctrScellConfigSuppressed + ctrScellConfigured) * 100`
*   **Promotion Accuracy**: `ctrBulkPromotions / (ctrBulkPromotions + ctrLatePromotions) * 100`

## Performance Counters

The counters capture the estimator's decisions and their timeliness. 

The safety counter to monitor closely is `ctrLatePromotions`. It increments when a burst that was ultimately classified as `BULK` had already delivered more than 20% of its volume before SCell activation completed (meaning the user experienced a throughput cost). A rising trend in this counter indicates a need to lower the `bulkBurstThr` parameter or shorten the `classDwellTimer` parameter.

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
| `ctrScellConfigured` | SCell configuration events executed | 0–2³¹ | int64 |
| `ctrScellConfigSuppressed` | SCell configurations avoided by demand classification | 0–2³¹ | int64 |
| `ctrBulkPromotions` | Timely promotions to `BULK` | 0–2³¹ | int64 |
| `ctrLatePromotions` | Promotions completing after 20% of burst served | 0–2³¹ | int64 |
| `ctrClassChanges` | Demand class transitions | 0–2³¹ | int64 |
| `ctrConnectedUeSamples` | Per-UE estimator samples | 0–2³¹ | int64 |
| `ctrPromotionDelayP95` | 95th percentile promotion delay per ROP (ms) | 0–10000 | int32 |

# Cross-References

*   [Parameters](parameters.md) — For configuring the `bulkBurstThr` and `classDwellTimer` parameters mentioned above.
*   [Feature Operation](feature-operation.md) — For details on the demand classification estimator and the definition of `BULK` bursts.
