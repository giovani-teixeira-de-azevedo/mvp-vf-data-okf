---
type: concept
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#performance-management
title: PERFORMANCE MANAGEMENT
description: Details performance management metrics, KPIs, and counters for monitoring
  device health and troubleshooting in Cascaded RET Support.
tags:
- performance management
- RET
- ALD
- AISG
- KPIs
- counters
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-29T16:02:51+00:00'
  source_sha256: 9264c8fbaa8efdb5
sources:
- resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
  title: Cascaded RET Support
---

This section details performance management for the Cascaded RET Support feature, addressing device health, communication continuity, and actuator operational metrics across AISG ports.

## Overview

Performance management for an antenna-line control feature answers questions of device health rather than traffic: are all expected actuators discovered and responsive, are tilt commands succeeding, and is any bus degrading (intermittent communication is the classic symptom of water ingress in a connector). Counters are collected per AISG port over the standard 15-minute ROP. Establish a baseline in the first week — a healthy bus shows zero communication failures over days — so that any non-zero failure rate stands out immediately.

## Key Performance Indicators (KPIs)

- **Tilt Success Rate**: Should sit at 100% in a healthy network; isolated failures correlate with movement timeouts on mechanically stiff actuators in cold weather and warrant a `movementTimeout` increase before a hardware ticket.
- **ALD Availability**: Below 99.9% on a specific port indicates a bus-level problem (connector, bias-tee, or cable) rather than a device fault, because a single-device fault leaves the other devices on the same bus responsive.
- **Scan Instability**: Rising Scan Instability means devices are dropping off and rejoining the bus — treat this as an early warning of connector corrosion.

### KPI Formulas

| KPI | Formula | Description |
| --- | --- | --- |
| Tilt Success Rate | `ctrRetTiltOk / (ctrRetTiltOk + ctrRetTiltFail) × 100` | Share of tilt commands completing successfully (%) |
| ALD Availability | `(1 − ctrAldCommFailTime / ctrAldSupervisedTime) × 100` | Time-based availability of supervised ALDs (%) |
| Scan Instability | `ctrAisgRescans / 96` | Unplanned bus rescans per ROP-day |

## Performance Counters

The counters measure command outcomes and supervision continuity. `ctrAldCommFail` deserves special attention: a low steady rate across many sites is often a firmware quirk of a specific actuator vendor (check the vendor code distribution), whereas a burst on one site is physical-layer trouble. `ctrRetTiltFail` should be correlated with ambient temperature before dispatching field service.

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

- [PARAMETERS](parameters.md)
