---
type: concept
resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf#parameters
title: PARAMETERS
description: Defines the configuration parameters for the Automated Energy Saver feature.
tags:
- energy-saver
- parameters
- configuration
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T18:22:27+00:00'
  source_sha256: 9c10e48e2adcd7fd
sources:
- title: Automated Energy Saver
  resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf
---

This section details the parameter set for the Automated Energy Saver feature. The parameter set is designed to be minimal, allowing operators to set high-level policy while automation handles operational execution.

## Parameter Overview

Most networks only set `savingsLevel` and the protected window (`protectedStart` and `protectedStop`). The `cellOptOut` attribute is set per cell and serves as the recommended handle for excluding special-event or VIP sites without needing to deactivate the feature node-wide.

## Parameter Reference Table

| Parameter | Description | Values | Datatype | Default |
| --- | --- | --- | --- | --- |
| `energySaverMode` | Enables automated control on the node | OFF, MONITOR, AUTO | enum | OFF |
| `savingsLevel` | Forecast margin policy | CONSERVATIVE, BALANCED, AGGRESSIVE | enum | BALANCED |
| `protectedStart` | Start of daily window with no automated actions | 00:00–23:59 | string | 06:30 |
| `protectedStop` | End of daily window with no automated actions | 00:00–23:59 | string | 22:00 |
| `wakeupLeadTime` | Pre-emptive wake-up lead ahead of forecast rise | 5–60 (min) | int32 | 30 |
| `cellOptOut` | Excludes the cell from automated control | true, false | boolean | false |
| `historyWindow` | Days of PM history used by the prediction model | 7–42 | int32 | 21 |
| `fallbackMode` | Behavior before sufficient history exists | DISABLED, REACTIVE_ONLY | enum | REACTIVE_ONLY |

# Cross-References

- [Feature Overview](feature-overview.md)
- [Deactivation Procedure](deactivation-procedure.md)
