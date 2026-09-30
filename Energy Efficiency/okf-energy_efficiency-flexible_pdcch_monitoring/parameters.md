---
type: concept
resource: data/vodafone-mvp/raw/Flexible PDCCH Monitoring.pdf#parameters
title: Parameters
description: Configuration parameters, value ranges, data types, and default settings
  for the Flexible PDCCH Monitoring feature.
tags:
- pdcch-monitoring
- parameters
- configuration
- sssg
- 5qi
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-30T14:20:09+00:00'
  source_sha256: b1fbfe543e0b13a7
sources:
- title: Flexible PDCCH Monitoring
  resource: data/vodafone-mvp/raw/Flexible PDCCH Monitoring.pdf
---

This section outlines the configuration parameters, value ranges, data types, and default settings used to tune and control the Flexible PDCCH Monitoring feature.

## Parameter Overview and Tuning Guidelines

The `sssgSwitchTimer` and `sparsePeriod` parameters form the primary tuning pair. A short `sssgSwitchTimer` maximizes energy saving but causes more frequent switching for chatty traffic; the default value suits smartphone mixes. The per-5QI profile table serves as the policy interface, where voice and interactive 5QIs should be kept on `FULL` or `ADAPTIVE`.

## Parameter Definitions

| Parameter | Description | Values | Datatype | Default |
| --- | --- | --- | --- | --- |
| `pdcchMonitoringMode` | Enables the function on the cell | `DISABLED`, `SSSG_ONLY`, `SSSG_AND_SKIP` | enum | `DISABLED` |
| `sparsePeriod` | SSSG1 monitoring periodicity | 2–16 (slots) | int32 | 4 |
| `sssgSwitchTimer` | Inactivity before switch to sparse group | 1–100 (ms) | int32 | 8 |
| `maxSkipDuration` | Maximum PDCCH skip window | 2–40 (ms) | int32 | 20 |
| `profile5qiMap` | 5QI-to-profile mapping | `FULL`, `ADAPTIVE`, `AGGRESSIVE` per 5QI | string | `"1:FULL,5:ADAPTIVE,9:AGGRESSIVE"` |
| `retxPinDense` | Pin UE to dense group while HARQ retx pending | `false`, `true` | boolean | `true` |
| `minUeReportGap` | Minimum gap preserved for CSI/SRS occasions | 0–20 (slots) | int32 | 4 |

# Cross-References

- [Feature Operation](feature-operation.md)
- [Activation Procedure](activation-procedure.md)
