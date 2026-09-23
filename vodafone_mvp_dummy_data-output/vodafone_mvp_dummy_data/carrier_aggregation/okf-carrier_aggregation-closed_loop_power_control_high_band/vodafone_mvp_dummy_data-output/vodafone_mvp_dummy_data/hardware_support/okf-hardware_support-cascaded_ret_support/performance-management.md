---
type: concept
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#performance-management
title: Performance Management
description: Performance management counters, KPIs, and formulas for monitoring device
  health and bus stability in Cascaded RET Support.
tags:
- performance-management
- kpi
- counters
- ret
- aisg
- ald
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T12:42:50+00:00'
  source_sha256: 9264c8fbaa8efdb5
sources:
- resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
  title: Cascaded RET Support
---

Performance management for the antenna-line control feature evaluates device health and bus degradation rather than network traffic. Counters are collected per AISG port over the standard 15-minute Result Output Period (ROP), with baseline measurements established during the first week.

## Key Performance Indicators

In a healthy network, Tilt Success Rate should sit at 100%. Isolated failures often correlate with movement timeouts on mechanically stiff actuators in cold weather and warrant increasing `movementTimeout` before opening a hardware ticket. ALD Availability below 99.9% on a specific port indicates a bus-level problem (such as connector, bias-tee, or cable issues) rather than a single device fault. Rising Scan Instability indicates devices dropping off and rejoining the bus, serving as an early warning of connector corrosion.

| KPI | Formula | Description |
| --- | --- | --- |
| Tilt Success Rate | `ctrRetTiltOk / (ctrRetTiltOk + ctrRetTiltFail) × 100` | Share of tilt commands completing successfully (%) |
| ALD Availability | `(1 − ctrAldCommFailTime / ctrAldSupervisedTime) × 100` | Time-based availability of supervised ALDs (%) |
| Scan Instability | `ctrAisgRescans / 96` | Unplanned bus rescans per ROP-day |

## Counters

Performance counters measure command outcomes and supervision continuity:

- **`ctrAldCommFail`**: A low, steady failure rate across multiple sites often indicates a firmware quirk of a specific actuator vendor, whereas a sudden burst on a single site points to physical-layer issues.
- **`ctrRetTiltFail`**: Should be correlated with ambient temperature prior to dispatching field service.

| Counter | Description | Range | Datatype |
| --- | --- | --- | --- |
| `ctrRetTiltOk` | Tilt commands completed successfully | 0–2³¹ | int64 |
| `ctrRetTiltFail` | Tilt commands failed or timed out | 0–2³¹ | int64 |
| `ctrAldCommFail` | ALD keep-alive failures detected | 0–2³¹ | int64 |
| `ctrAldCommFailTime` | Accumulated time ALDs unreachable per ROP | 0–900 s | int64 |
| `ctrAldSupervisedTime` | Accumulated supervised device-time per ROP | 0–2³¹ s | int64 |
| `ctrAisgRescans` | Unplanned bus rescans triggered | 0–2³¹ | int64 |
| `ctrCalibrationFail` | Calibration attempts that failed | 0–2³¹ | int64 |

# Cross-References

- [Parameters](parameters.md) — Describes feature configuration parameters including `movementTimeout`.
