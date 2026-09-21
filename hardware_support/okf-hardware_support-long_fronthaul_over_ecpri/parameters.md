---
type: reference-table
resource: data/vodafone-mvp/raw/Long Fronthaul over eCPRI.pdf#parameters
title: Parameters
description: Configuration parameters setting the delay budget contract between the
  radio and transport domains for Long Fronthaul over eCPRI.
tags:
- parameters
- delay-budget
- configuration
- long-fronthaul
- ecpri
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:45:18+00:00'
  source_sha256: 3f805e438017e709
sources:
- resource: data/vodafone-mvp/raw/Long Fronthaul over eCPRI.pdf
  title: Long Fronthaul over eCPRI
---

This section defines the configuration parameters that establish the delay budget contract between the radio and transport domains. 

The parameter `maxOneWayDelay` is the declared design value that validation checks are performed against. This parameter must be configured based on the transport design rather than the first measured value, ensuring that protection-path switches (which lengthen the transmission path) are already factored into the budget.

| Parameter | Description | Values | Datatype | Default |
| :--- | :--- | :--- | :--- | :--- |
| `longFronthaulMode` | Enables extended delay operation per fronthaul port | `DISABLED`, `ENABLED` | `enum` | `DISABLED` |
| `maxOneWayDelay` | Designed maximum one-way path delay | `30–200` (µs) | `int32` | `100` |
| `delayDriftTolerance` | Allowed drift before re-validation | `1–20` (µs) | `int32` | `5` |
| `delayMeasInterval` | In-service delay measurement period | `1–60` (s) | `int32` | `10` |
| `jitterBudget` | Max accepted packet delay variation | `1–10` (µs) | `int32` | `5` |
| `harqBudgetPolicy` | Behavior when budget exceeded | `BAR_CELLS`, `ALARM_ONLY` | `enum` | `BAR_CELLS` |
| `bufferHeadroom` | Extra buffer beyond measured delay | `5–50` (µs) | `int32` | `10` |
| `asymmetryCompensation` | Manual TX/RX fiber asymmetry correction | `−5000–5000` (ns) | `int32` | `0` |

# Cross-References

* [Feature Operation](feature-operation.md) — Describes the execution and validation of delay budgets using these parameters.
* [Activation Procedure](activation-procedure.md) — Describes how to enable and configure these parameters on the network elements.
