---
type: reference-table
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#parameters
title: Parameters
description: Configuration parameters governing AISG bus scanning behavior and per-device
  antenna tilt control in Cascaded RET Support.
tags:
- cascaded-ret
- parameters
- aisg
- tilt-control
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T12:42:46+00:00'
  source_sha256: 22c6c6d58bac6fbe
sources:
- title: Cascaded RET Support
  resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
---

This section details the configuration parameters for Cascaded RET Support, governing AISG bus scanning behavior and per-device antenna tilt control.

The parameters govern bus scanning behavior and per-device tilt control. Defaults are safe for standard installations; `aisgScanMode` is the only parameter most deployments touch, switching from manual to automatic scanning after the site integration phase is complete so that replaced actuators are rediscovered without operator action.

## Parameter Summary

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
