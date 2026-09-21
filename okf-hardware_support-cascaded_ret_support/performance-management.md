---
type: concept
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#performance-management
title: Performance Management
description: Performance management, KPIs, and counter definitions for evaluating
  the device health and communication stability of cascaded RET and ALD systems.
tags:
- PM
- KPI
- Counters
- AISG
- RET
- ALD
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:42:45+00:00'
  source_sha256: 9264c8fbaa8efdb5
sources:
- resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
  title: Cascaded RET Support
---

Performance management for the antenna-line control feature focuses on monitoring device health and hardware communication integrity rather than traffic-related performance. It tracks the discovery, responsiveness, and command outcomes of Remote Electrical Tilt (RET) actuators and Antenna Line Device (ALD) systems, as well as the stability of the AISG bus.

## Performance Monitoring Strategy

Counters are collected per AISG port over a standard 15-minute Reporting Period (ROP). 

* **Baseline Establishment:** It is recommended to establish a baseline in the first week of operation. A healthy bus should show zero communication failures over multiple days, allowing any subsequent non-zero failure rate to stand out immediately.

## Key Performance Indicators (KPIs)

| KPI | Formula | Description | Interpretation & Troubleshooting |
| :--- | :--- | :--- | :--- |
| **Tilt Success Rate** | `ctrRetTiltOk / (ctrRetTiltOk + ctrRetTiltFail) * 100` | Share of tilt commands completing successfully (%) | Should sit at 100% in a healthy network. Isolated failures correlate with movement timeouts on mechanically stiff actuators in cold weather, and warrant a `movementTimeout` parameter increase before opening a hardware ticket. |
| **ALD Availability** | `(1 − ctrAldCommFailTime / ctrAldSupervisedTime) * 100` | Time-based availability of supervised ALDs (%) | Below 99.9% on a specific port indicates a bus-level problem (such as a connector, bias-tee, or cable issue) rather than a device fault. This is because a single-device fault leaves other devices on the same bus responsive. |
| **Scan Instability** | `ctrAisgRescans / 96` | Unplanned bus rescans per ROP-day | Rising scan instability means devices are dropping off and rejoining the bus. This should be treated as an early warning of connector corrosion (e.g., from water ingress in a connector). |

## Counters

The performance counters measure command outcomes and supervision continuity:
* **ctrAldCommFail:** A low, steady rate across many sites is often a firmware quirk of a specific actuator vendor (which can be verified by checking the vendor code distribution), whereas a burst on one site indicates physical-layer trouble.
* **ctrRetTiltFail:** This counter should be correlated with ambient temperature before dispatching field service, as cold weather can cause mechanically stiff actuators to time out.

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
| `ctrRetTiltOk` | Tilt commands completed successfully | 0–2³¹ | `int64` |
| `ctrRetTiltFail` | Tilt commands failed or timed out | 0–2³¹ | `int64` |
| `ctrAldCommFail` | ALD keep-alive failures detected | 0–2³¹ | `int64` |
| `ctrAldCommFailTime` | Accumulated time ALDs unreachable per ROP | 0–900 s | `int64` |
| `ctrAldSupervisedTime` | Accumulated supervised device-time per ROP | 0–2³¹ s | `int64` |
| `ctrAisgRescans` | Unplanned bus rescans triggered | 0–2³¹ | `int64` |
| `ctrCalibrationFail` | Calibration attempts that failed | 0–2³¹ | `int64` |

## Cross-References

* [Parameters](parameters.md) — For details on the `movementTimeout` parameter.
