---
type: reference-table
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#parameters
title: PARAMETERS
description: Parameters governing bus scanning behavior and per-device tilt control
  for Cascaded RET Support.
tags:
- parameters
- RET
- AISG
- configuration
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:42:50+00:00'
  source_sha256: 22c6c6d58bac6fbe
sources:
- title: Cascaded RET Support
  resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
---

This section defines the configuration parameters for the Cascaded RET Support feature. These parameters govern bus scanning behavior and per-device tilt control.

Defaults are safe for standard installations; `aisgScanMode` is the only parameter most deployments touch. Switching this parameter from manual to automatic scanning after the site integration phase is complete ensures that replaced actuators are rediscovered without operator action.

## Parameter Reference

| Parameter | Description | Values | Datatype | Default |
| :--- | :--- | :--- | :--- | :--- |
| `aisgScanMode` | Bus scan trigger mode per port | `MANUAL`<br>`AUTO_ON_UNLOCK`<br>`PERIODIC` | enum | `AUTO_ON_UNLOCK` |
| `aisgScanInterval` | Rescan interval when `PERIODIC` | 1–168 (h) | int32 | 24 |
| `maxAldPerPort` | Administrative cap on discovered ALDs per port | 1–12 | int32 | 12 |
| `electricalTilt` | Commanded tilt per `RetDevice` subunit | device range (0.1°) | int32 | 0 |
| `retSupervisionTimer` | Keep-alive poll interval per device | 10–600 (s) | int32 | 60 |
| `busPowerMode` | DC power feed on the AISG port | `OFF`<br>`ON`<br>`ON_DEMAND` | enum | `ON` |
| `movementTimeout` | Max time a tilt movement may run | 30–600 (s) | int32 | 180 |
| `calibrationRequired` | Reject tilt commands on uncalibrated devices | `true`, `false` | boolean | `true` |

# Cross-References

* [Feature Overview](feature-overview.md) - For context on Cascaded RET Support.
* [Feature Operation](feature-operation.md) - For details on how these scanning and control parameters are used in operation.
