---
type: concept
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#performance-management
title: Performance Management
description: Defines performance management principles, KPIs, and PM counters for
  monitoring Cascaded RET hardware health and AISG bus integrity.
tags:
- performance-management
- kpi
- counters
- aisg
- ret
- cascaded-ret
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T18:53:02+00:00'
  source_sha256: 9264c8fbaa8efdb5
sources:
- resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
  title: Cascaded RET Support
---

The **Performance Management** section describes device health monitoring, Key Performance Indicators (KPIs), and counters for the Cascaded Remote Electrical Tilt (RET) feature. Performance management focuses on antenna-line control device health and communication integrity per AISG port over 15-minute Report Output Periods (ROPs).

## Overview

Performance management for antenna-line control monitors device health rather than user traffic. Key health indicators include:
* Actuator discovery and responsiveness.
* Tilt command success rates.
* Bus degradation detection (e.g., intermittent communication caused by connector water ingress).

Counters are collected per AISG port over the standard 15-minute ROP. A baseline should be established during the first week; a healthy bus exhibits zero communication failures over multiple days, allowing any non-zero failure rate to be identified immediately.

## Key Performance Indicators (KPIs)

* **Tilt Success Rate**: Expected to be 100% in a healthy network. Isolated failures often correlate with movement timeouts on mechanically stiff actuators in cold weather, which may warrant increasing `movementTimeout` prior to submitting a hardware ticket.
* **ALD Availability**: An ALD availability below 99.9% on a specific port indicates a bus-level failure (connector, bias-tee, or cable) rather than an individual device fault, as a single-device failure leaves other devices on the same bus responsive.
* **Scan Instability**: A rising scan instability metric indicates devices dropping off and rejoining the bus, serving as an early warning of connector corrosion.

| KPI | Formula | Description |
| --- | --- | --- |
| **Tilt Success Rate** | `ctrRetTiltOk / (ctrRetTiltOk + ctrRetTiltFail) × 100` | Share of tilt commands completing successfully (%) |
| **ALD Availability** | `(1 − ctrAldCommFailTime / ctrAldSupervisedTime) × 100` | Time-based availability of supervised ALDs (%) |
| **Scan Instability** | `ctrAisgRescans / 96` | Unplanned bus rescans per ROP-day |

## Counters

Counters measure command outcomes and supervision continuity across the AISG interface.
* `ctrAldCommFail`: A low, steady rate across many sites often indicates a firmware quirk of a specific actuator vendor (check vendor code distribution). A burst on a single site indicates physical-layer trouble.
* `ctrRetTiltFail`: Should be correlated with ambient temperature before dispatching field service.

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

* [Parameters](parameters.md) – Covers configuration parameters including `movementTimeout`.
