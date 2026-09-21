---
type: concept
resource: data/vodafone-mvp/raw/Extended Propagation Delay Support High-Band.pdf#performance-management
title: Performance Management
description: Performance management guidelines, KPIs, and PM counters for validating
  extended propagation delay support.
tags:
- performance-management
- kpis
- counters
- extended-range
- prach
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:27:05+00:00'
  source_sha256: 5935f7d84869d8b6
sources:
- title: Extended Propagation Delay Support High-Band
  resource: data/vodafone-mvp/raw/Extended Propagation Delay Support High-Band.pdf
---

This section details the performance management parameters, KPI formulas, and Performance Measurement (PM) counters used to validate the activation of Extended Propagation Delay Support. These metrics evaluate accessibility for distant User Equipments (UEs) and monitor potential network impacts such as false PRACH detections.

## Overview

Performance management validates that the range extension delivers accessibility for distant UEs without degrading the cell for near ones. It answers two primary questions:
1. Are distant UEs succeeding in random access and SCell addition?
2. Has the wider window introduced interference or false-preamble cost?

### Baseline Monitoring
Before activation, establish a baseline for:
- Baseline Random Access (RA) success rate
- False PRACH detections
- Uplink SINR distribution

All counters are collected per cell per 15-minute Reporting Period (ROP).

---

## Key Performance Indicators (KPIs)

*   **Distant RA Success**: Should rise to within a few points of the overall RA success rate. A persistent gap indicates that the link budget, and not timing, is the limiter (in which case, check CPE alignment).
*   **False Preamble Rate**: Should stay below 0.5%. The wider window statistically admits more noise-triggered detections, and values above 1% call for reviewing the PRACH threshold or reducing the configured max cell range.
*   **Extended Range Utilization**: Indicates whether the feature is earning its overhead. If it stays near zero for weeks, the cell has no distant population and the feature can be disabled.
*   **SCell Ext-Range Addition Success**: Success rate of SCell additions requiring extended Timing Advance (TA) (%).

### KPI Formulas

| KPI | Formula | Description |
| :--- | :--- | :--- |
| **Distant RA Success** | `ctrRaSuccessExtRange / ctrRaAttemptExtRange × 100` | RA success for attempts with TA beyond the legacy limit (%) |
| **Extended Range Utilization** | `ctrUeExtRangeSamples / ctrUeTaSamples × 100` | Share of TA samples beyond the legacy limit (%) |
| **False Preamble Rate** | `ctrPrachFalseDetect / ctrPrachDetectTotal × 100` | Share of PRACH detections classified as false (%) |
| **SCell Ext-Range Addition Success** | `ctrScellAddExtRangeSucc / ctrScellAddExtRangeAtt × 100` | Success rate of SCell additions requiring extended TA (%) |

---

## PM Counters

Specific counters provide insights into range limitations and distribution:

*   **`ctrUeBeyondMaxRange`**: Counts admission rejections of UEs whose TA exceeds the configured maximum cell range. A steady non-zero value indicates that real subscribers sit just outside the configured range; consider raising the maximum cell range if the link budget supports it.
*   **TA Histogram Counters (`ctrTaHistBin01` to `ctrTaHistBinNN`)**: One per configured bin. These counters feed range-distribution analysis and are exported as a vector counter.

### Counter Reference

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
| `ctrRaAttemptExtRange` | RA attempts with delay beyond legacy limit | 0–2³¹ | int64 |
| `ctrRaSuccessExtRange` | Successful RA beyond legacy limit | 0–2³¹ | int64 |
| `ctrUeTaSamples` | TA samples collected | 0–2³¹ | int64 |
| `ctrUeExtRangeSamples` | TA samples beyond legacy limit | 0–2³¹ | int64 |
| `ctrPrachDetectTotal` | Total PRACH detections | 0–2³¹ | int64 |
| `ctrPrachFalseDetect` | PRACH detections classified false | 0–2³¹ | int64 |
| `ctrScellAddExtRangeAtt` | SCell additions attempted needing extended TA | 0–2³¹ | int64 |
| `ctrScellAddExtRangeSucc` | Such SCell additions succeeding | 0–2³¹ | int64 |
| `ctrUeBeyondMaxRange` | Admissions rejected for exceeding maxCellRange | 0–2³¹ | int64 |

# Cross-References

* [Parameters](parameters.md) — For configuration details related to maximum cell range and PRACH threshold limits.
