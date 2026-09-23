---
type: concept
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#parameters
title: PARAMETERS
description: Configuration parameters governing bus scanning behavior and per-device
  tilt control for Cascaded RET Support.
tags:
- RET
- AISG
- parameters
- configuration
- tilt-control
- bus-scanning
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T15:10:30+00:00'
  source_sha256: 22c6c6d58bac6fbe
sources:
- resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
  title: Cascaded RET Support
---

This section details the configuration parameters governing bus scanning behavior and per-device tilt control within the Cascaded RET Support feature.

Defaults are safe for standard installations; `aisgScanMode` is the only parameter most deployments touch, switching from manual to automatic scanning after the site integration phase is complete so that replaced actuators are rediscovered without operator action.

| Parameter | Description | Values | Datatype | Default |
| --- | --- | --- | --- | --- |
| `aisgScanMode` | Bus scan trigger mode per port | MANUAL, AUTO_ON_UNLOCK, PERIODIC | enum | AUTO_ON_UNLOCK |
| `aisgScanInterval` | Rescan interval when PERIODIC | 1–168 (h) | int32 | 24 |
| `maxAldPerPort` | Administrative cap on discovered ALDs per port | 1–12 | int32 | 12 |
| `electricalTilt` | Commanded tilt per RetDevice subunit | device range (0.1°) | int32 | 0 |
| `retSupervisionTimer` | Keep-alive poll interval per device | 10–600 (s) | int32 | 60 |
| `busPowerMode` | DC power feed on the AISG port | OFF, ON, ON_DEMAND | enum | ON |
| `movementTimeout` | Max time a tilt movement may run | 30–600 (s) | int32 | 180 |
| `calibrationRequired` | Reject tilt commands on uncalibrated devices | true, false | boolean | true |

# Cross-References

- [Feature Operation](feature-operation.md)
- [Activation Procedure](activation-procedure.md)
