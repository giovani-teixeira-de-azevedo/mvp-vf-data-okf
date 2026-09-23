---
type: concept
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#performance-management
title: Performance Management
description: Provides performance management guidelines, KPIs, and counters for evaluating
  device health, tilt operation, and bus stability in Cascaded RET Support.
tags:
- RET
- AISG
- Performance Management
- KPIs
- Counters
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T07:59:47+00:00'
  source_sha256: 9264c8fbaa8efdb5
sources:
- resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
  title: Cascaded RET Support
---

Performance management for the Cascaded RET Support feature focuses on device health rather than traffic metrics, assessing whether expected actuators are discovered and responsive, tilt commands succeed, and bus communication remains stable. Counters are collected per AISG port over a standard 15-minute Result Output Period (ROP). Establishing a baseline during the first week allows non-zero communication failure rates to be identified quickly.

## Key Performance Indicators (KPIs)

- **Tilt Success Rate**: Should sit at 100% in a healthy network. Isolated failures correlate with movement timeouts on mechanically stiff actuators in cold weather and warrant increasing `movementTimeout` before issuing a hardware ticket.
- **ALD Availability**: Availability below 99.9% on a specific port indicates a bus-level problem (such as a connector, bias-tee, or cable) rather than a device fault, as a single-device fault leaves other devices on the same bus responsive.
- **Scan Instability**: A rising scan instability indicates devices dropping off and rejoining the bus, serving as an early warning of connector corrosion.

| KPI | Formula | Description |
| --- | --- | --- |
| Tilt Success Rate | `ctrRetTiltOk / (ctrRetTiltOk + ctrRetTiltFail) × 100` | Share of tilt commands completing successfully (%) |
| ALD Availability | `(1 − ctrAldCommFailTime / ctrAldSupervisedTime) × 100` | Time-based availability of supervised ALDs (%) |
| Scan Instability | `ctrAisgRescans / 96` | Unplanned bus rescans per ROP-day |

## Counters

Counters measure command outcomes and supervision continuity:
- `ctrAldCommFail`: A low steady rate across many sites is often a firmware quirk of a specific actuator vendor (checked via vendor code distribution), whereas a burst on one site indicates physical-layer trouble.
- `ctrRetTiltFail`: Should be correlated with ambient temperature before dispatching field service.

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

- [Parameters](parameters.md)
