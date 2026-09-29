---
type: reference-table
resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf#parameters
title: PARAMETERS
description: Configuration parameters for controlling Automated Energy Saver node
  and cell behavior.
tags:
- parameters
- automated-energy-saver
- energy-saver
- configuration
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-29T22:58:52+00:00'
  source_sha256: 9c10e48e2adcd7fd
sources:
- title: Automated Energy Saver
  resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf
---

This section details the configuration parameters for the Automated Energy Saver feature. The parameter set is deliberately small, requiring policy input while allowing automation output. Most networks only set `savingsLevel` and the protected window, using the cell-level opt-out attribute to exclude special-event or VIP sites rather than deactivating the feature node-wide.

## Parameter Reference

| Parameter | Description | Values | Datatype | Default |
| --- | --- | --- | --- | --- |
| `energySaverMode` | Enables automated control on the node | `OFF`, `MONITOR`, `AUTO` | enum | `OFF` |
| `savingsLevel` | Forecast margin policy | `CONSERVATIVE`, `BALANCED`, `AGGRESSIVE` | enum | `BALANCED` |
| `protectedStart` | Start of daily window with no automated actions | 00:00–23:59 | string | 06:30 |
| `protectedStop` | End of daily window with no automated actions | 00:00–23:59 | string | 22:00 |
| `wakeupLeadTime` | Pre-emptive wake-up lead ahead of forecast rise | 5–60 (min) | int32 | 30 |
| `cellOptOut` | Excludes the cell from automated control | `true`, `false` | boolean | `false` |
| `historyWindow` | Days of PM history used by the prediction model | 7–42 | int32 | 21 |
| `fallbackMode` | Behavior before sufficient history exists | `DISABLED`, `REACTIVE_ONLY` | enum | `REACTIVE_ONLY` |
