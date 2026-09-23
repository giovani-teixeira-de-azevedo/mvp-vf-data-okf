---
type: concept
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#performance-management
title: PERFORMANCE MANAGEMENT
description: Performance management, Key Performance Indicators (KPIs), formulas,
  and counters for monitoring device health and AISG bus status in Cascaded RET Support.
tags:
- aisg
- ald
- ret
- performance-management
- kpi
- counters
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T10:08:38+00:00'
  source_sha256: 9264c8fbaa8efdb5
sources:
- title: Cascaded RET Support
  resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
---

Performance management for the Cascaded RET Support feature focuses on monitoring antenna-line device health and AISG bus integrity rather than traffic performance.

## Overview

Performance management for an antenna-line control feature answers questions of device health rather than traffic: whether all expected actuators are discovered and responsive, tilt commands are succeeding, and any bus is degrading (where intermittent communication is the classic symptom of water ingress in a connector). Counters are collected per AISG port over the standard 15-minute Result Output Period (ROP). Establishing a baseline in the first week—where a healthy bus shows zero communication failures over days—ensures that any non-zero failure rate stands out immediately.

## Key Performance Indicators (KPIs)

- **Tilt Success Rate**: Should sit at 100% in a healthy network. Isolated failures correlate with movement timeouts on mechanically stiff actuators in cold weather and warrant a `movementTimeout` increase before a hardware ticket is issued.
- **ALD Availability**: Below 99.9% on a specific port indicates a bus-level problem (connector, bias-tee, or cable) rather than a device fault, because a single-device fault leaves the other devices on the same bus responsive.
- **Scan Instability**: Rising scan instability means devices are dropping off and rejoining the bus, serving as an early warning of connector corrosion.

### KPI Formulas

| KPI | Formula | Description |
|---|---|---|
| Tilt Success Rate | `ctrRetTiltOk / (ctrRetTiltOk + ctrRetTiltFail) × 100` | Share of tilt commands completing successfully (%) |
| ALD Availability | `(1 − ctrAldCommFailTime / ctrAldSupervisedTime) × 100` | Time-based availability of supervised ALDs (%) |
| Scan Instability | `ctrAisgRescans / 96` | Unplanned bus rescans per ROP-day |

## Performance Counters

The performance counters measure command outcomes and supervision continuity:
- `ctrAldCommFail`: Deserves special attention; a low steady rate across many sites is often a firmware quirk of a specific actuator vendor (check the vendor code distribution), whereas a burst on one site indicates physical-layer trouble.
- `ctrRetTiltFail`: Should be correlated with ambient temperature before dispatching field service.

### Counter Definitions

| Counter | Description | Range | Datatype |
|---|---|---|---|
| `ctrRetTiltOk` | Tilt commands completed successfully | 0–2³¹ | int64 |
| `ctrRetTiltFail` | Tilt commands failed or timed out | 0–2³¹ | int64 |
| `ctrAldCommFail` | ALD keep-alive failures detected | 0–2³¹ | int64 |
| `ctrAldCommFailTime` | Accumulated time ALDs unreachable per ROP | 0–900 s | int64 |
| `ctrAldSupervisedTime` | Accumulated supervised device-time per ROP | 0–2³¹ s | int64 |
| `ctrAisgRescans` | Unplanned bus rescans triggered | 0–2³¹ | int64 |
| `ctrCalibrationFail` | Calibration attempts that failed | 0–2³¹ | int64 |

# Cross-References

- [Parameters](parameters.md) - Documents configuration parameters including `movementTimeout`.
