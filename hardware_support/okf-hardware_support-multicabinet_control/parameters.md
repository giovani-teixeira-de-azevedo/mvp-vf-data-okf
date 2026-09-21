---
type: reference-table
resource: data/vodafone-mvp/raw/Multicabinet Control.pdf#parameters
title: Parameters
description: Configuration parameters for Cabinet Managed Objects (MO) with node-level
  coordination settings.
tags:
- parameters
- cabinet-mo
- multicabinet-control
- load-shedding
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:45:57+00:00'
  source_sha256: dbf697dcc9541cb7
sources:
- resource: data/vodafone-mvp/raw/Multicabinet Control.pdf
  title: Multicabinet Control
---

Parameters are set per Cabinet MO with node-level coordination settings. The load-shed priority list is the parameter with real service consequences — review it with the radio planning team, since it decides which cells stay up in hour three of a mains outage.

| Parameter | Description | Values | Datatype | Default |
| :--- | :--- | :--- | :--- | :--- |
| `cabinetRole` | Role of the cabinet in the site assembly | `PRIMARY, SECONDARY` | enum | `SECONDARY` |
| `boundScuSerial` | Serial number binding for the cabinet's SCU | string | string | `""` |
| `climateSetpoint` | Target internal temperature | 10–40 (°C) | int32 | 25 |
| `climateCoordination` | Cross-cabinet setpoint balancing | `OFF, ON` | enum | `ON` |
| `loadShedPriority` | Shed order of this cabinet's non-critical loads | 1–8 (1 = shed first) | int32 | 4 |
| `batteryTestSchedule` | Periodic battery test cron expression | string | string | `"0 3 1  "` |
| `sensorPollInterval` | Sensor data collection period | 1–60 (s) | int32 | 1 |
| `doorAlarmEnabled` | Door-open alarm per cabinet | `true, false` | boolean | `true` |
| `scuLossPolicy` | Behavior on secondary SCU comms loss | `ALARM_ONLY, ALARM_AND_ESCALATE` | enum | `ALARM_ONLY` |

# Cross-References

- [Feature Operation](feature-operation.md)
- [Network Impact](network-impact.md)
