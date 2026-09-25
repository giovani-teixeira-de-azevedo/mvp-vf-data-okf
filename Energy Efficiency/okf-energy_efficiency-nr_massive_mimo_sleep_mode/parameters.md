---
type: reference-table
resource: data/vodafone-mvp/raw/NR Massive MIMO Sleep Mode.pdf#parameters
title: PARAMETERS
description: Configuration parameters for tuning NR Massive MIMO Sleep Mode on a per-cell
  basis.
tags:
- NR
- Massive MIMO
- Sleep Mode
- Parameters
- Configuration
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-25T16:09:41+00:00'
  source_sha256: 286c7b8b50faf0c0
sources:
- title: NR Massive MIMO Sleep Mode
  resource: data/vodafone-mvp/raw/NR Massive MIMO Sleep Mode.pdf
---

This section details the per-NR-cell configuration parameters used to tune the NR Massive MIMO Sleep Mode feature.

The feature is tuned per NR cell through the parameters below. The enter/exit threshold pairs are deliberately asymmetric (hysteresis) to avoid oscillation between sleep and full operation; keep `prbLoadExitThr` at least 10 percentage points above `prbLoadEnterThr`. Conservative defaults are provided — most networks only need to adjust the thresholds and the time-of-day window.

| Parameter | Description | Values | Datatype | Default |
| --- | --- | --- | --- | --- |
| `sleepMode` | Enables the function on the cell | `DISABLED`, `PARTIAL_ONLY`, `PARTIAL_AND_DEEP` | enum | `DISABLED` |
| `prbLoadEnterThr` | DL PRB utilization below which sleep entry is evaluated | 0–100 (%) | int32 | 10 |
| `prbLoadExitThr` | DL PRB utilization above which sleep is exited | 0–100 (%) | int32 | 25 |
| `connUsersEnterThr` | Max RRC-connected UEs allowed for sleep entry | 0–1000 | int32 | 20 |
| `sleepEnterTimer` | Time load must stay below thresholds before entry | 60–3600 (s) | int32 | 300 |
| `sleepExitTimer` | Guard time before re-entry after an exit | 0–3600 (s) | int32 | 600 |
| `sleepAllowedStart` | Start of daily window in which sleep is permitted | 00:00–23:59 | string | 00:00 |
| `sleepAllowedStop` | End of daily window in which sleep is permitted | 00:00–23:59 | string | 23:59 |

# Cross-References

- [FEATURE OPERATION](feature-operation.md)
- [ACTIVATION PROCEDURE](activation-procedure.md)
