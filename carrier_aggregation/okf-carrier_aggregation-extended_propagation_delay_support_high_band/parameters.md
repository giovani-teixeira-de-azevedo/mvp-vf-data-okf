---
type: reference-table
resource: data/vodafone-mvp/raw/Extended Propagation Delay Support High-Band.pdf#parameters
title: Parameters
description: Configuration parameters for Extended Propagation Delay Support High-Band.
tags:
- parameters
- configuration
- FR2
- propagation-delay
- PRACH
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:26:55+00:00'
  source_sha256: 4f15ba8652949c4b
sources:
- resource: data/vodafone-mvp/raw/Extended Propagation Delay Support High-Band.pdf
  title: Extended Propagation Delay Support High-Band
---

This section outlines the configuration parameters set per FR2 sector carrier for the Extended Propagation Delay Support High-Band feature. 

The parameter `maxCellRange` serves as the master control knob, from which the PRACH format, window sizes, and maximum Timing Advance (TA) are automatically derived. The remaining parameters are utilized to fine-tune network admission and performance management observability behaviors.

### Parameter Definitions

| Parameter | Description | Values | Datatype | Default |
| :--- | :--- | :--- | :--- | :--- |
| `extendedRangeMode` | Enables extended propagation delay support | `DISABLED`, `ENABLED` | enum | `DISABLED` |
| `maxCellRange` | Maximum planned cell range in meters (m) | 2000–10000 | int32 | 5000 |
| `prachFormatExt` | Extended PRACH format selection | `AUTO`, `B4`, `C2` | enum | `AUTO` |
| `taAdmissionMargin` | Margin below max TA at which new UEs are still admitted in meters (m) | 0–1000 | int32 | 200 |
| `distantUeReleaseCause` | Distinct release cause code for out-of-range UEs | 0–255 | int32 | 46 |
| `rangeHistogramBins` | Number of TA histogram bins for Performance Management (PM) | 4–32 | int32 | 16 |
| `scellExtRangeAllowed` | Allow extended TA at SCell addition | `true`, `false` | boolean | `true` |

# Cross-References

* [Feature Operation](feature-operation.md) — For details on how `maxCellRange` acts as the master knob for PRACH format, window sizes, and TA derivation.
* [Performance Management](performance-management.md) — For information on how the `rangeHistogramBins` parameter affects PM telemetry and TA histogram binning.
