---
type: reference-table
resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf#parameters
title: Parameters
description: Configuration parameters for controlling automated energy saver behavior
  on the node and cell levels.
tags:
- automated-energy-saver
- parameters
- configuration
- ran-tuning
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:38:23+00:00'
  source_sha256: 9c10e48e2adcd7fd
sources:
- title: Automated Energy Saver
  resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf
---

This section defines the configuration parameters used to manage the Automated Energy Saver feature. The parameter set is deliberately small, focusing on high-level policy input while automated control handles execution.

Most networks only need to configure the `savingsLevel` and the protected window (`protectedStart` and `protectedStop`). The `cellOptOut` attribute is configured per cell and is the recommended handle for excluding special-event or VIP sites from automated control, rather than deactivating the feature node-wide.

## Parameter Reference

| Parameter | Description | Values | Datatype | Default |
| :--- | :--- | :--- | :--- | :--- |
| `energySaverMode` | Enables automated control on the node | OFF, MONITOR, AUTO | enum | OFF |
| `savingsLevel` | Forecast margin policy | CONSERVATIVE, BALANCED, AGGRESSIVE | enum | BALANCED |
| `protectedStart` | Start of daily window with no automated actions | 00:00–23:59 | string | 06:30 |
| `protectedStop` | End of daily window with no automated actions | 00:00–23:59 | string | 22:00 |
| `wakeupLeadTime` | Pre-emptive wake-up lead ahead of forecast rise | 5–60 (min) | int32 | 30 |
| `cellOptOut` | Excludes the cell from automated control | true, false | boolean | false |
| `historyWindow` | Days of PM history used by the prediction model | 7–42 | int32 | 21 |
| `fallbackMode` | Behavior before sufficient history exists | DISABLED, REACTIVE_ONLY | enum | REACTIVE_ONLY |

# Cross-References

* [Feature Overview](feature-overview.md)
* [Feature Operation](feature-operation.md)
* [Activation Procedure](activation-procedure.md)
