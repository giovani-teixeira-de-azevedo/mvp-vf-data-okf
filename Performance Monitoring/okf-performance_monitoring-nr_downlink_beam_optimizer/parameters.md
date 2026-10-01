---
type: reference-table
resource: data/vodafone-mvp/raw/NR Downlink Beam Optimizer.pdf#parameters
title: Parameters
description: Configuration parameters for the NR Downlink Beam Optimizer, located
  per cell under the BeamOptimizer MO.
tags:
- parameters
- configuration
- beam-optimizer
- nr
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T14:20:56+00:00'
  source_sha256: d6a11b56ddbd3ed3
sources:
- resource: data/vodafone-mvp/raw/NR Downlink Beam Optimizer.pdf
  title: NR Downlink Beam Optimizer
---

This section details the configuration parameters for the NR Downlink Beam Optimizer. These parameters are configured per cell under the Managed Object (MO) path `NodeRoot=1,NrFunction=1,NrCell=<id>,BeamOptimizer=1`.

The default parameter values implement a cautious, slow loop with automatic rollback. Most operators start by configuring the optimizer in `OPEN_LOOP` mode and transition to `CLOSED_LOOP` mode after gaining confidence in the system's behavior.

## Parameter Reference Table

| Parameter | Description | Values | Datatype | Default |
| :--- | :--- | :--- | :--- | :--- |
| `optimizerMode` | Operating mode of the optimizer | `DISABLED`, `OPEN_LOOP`, `CLOSED_LOOP` | enum | `DISABLED` |
| `evalPeriod` | Hours of data aggregated per evaluation cycle | 6–168 (h) | int32 | 24 |
| `minChangeInterval` | Minimum time between applied grid changes | 24–720 (h) | int32 | 24 |
| `changeHysteresis` | Score margin required to change grid | 0.5–3.0 (dB) | decimal | 1.0 |
| `minSampleCount` | Minimum beam samples per cycle to optimize | 1000–1000000 | int32 | 50000 |
| `edgeRsrpTarget` | Cell-edge SS-RSRP the new grid must preserve | -140 to -44 (dBm) | int32 | -110 |
| `changeWindowStart` | Daily window start for applying changes | 00:00–23:59 | string | 02:00 |
| `changeWindowStop` | Daily window stop for applying changes | 00:00–23:59 | string | 05:00 |
| `guardPeriod` | Post-change accessibility watch duration | 1–48 (h) | int32 | 24 |
| `rollbackThreshold` | Accessibility drop triggering auto-rollback | 0.5–10.0 (pp) | decimal | 2.0 |

# Cross-References

* [Feature Operation](feature-operation.md) — Details on how these parameters affect the evaluation and rollback loops.
* [Activation Procedure](activation-procedure.md) — Step-by-step instructions on configuring these parameters during activation.
