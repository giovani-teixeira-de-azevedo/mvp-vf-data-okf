---
type: concept
resource: data/vodafone-mvp/raw/Multicabinet Control.pdf#performance-management
title: PERFORMANCE MANAGEMENT
description: Performance management guidelines, KPIs, and counter definitions for
  site support hardware and multicabinet control.
tags:
- performance-management
- kpis
- counters
- multicabinet-control
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:45:40+00:00'
  source_sha256: c7bf25b57c0d3572
sources:
- title: Multicabinet Control
  resource: data/vodafone-mvp/raw/Multicabinet Control.pdf
---

This section defines the performance management (PM) guidelines, KPIs, and counter definitions for multicabinet site support hardware. PM analysis for these elements is trend-oriented, focusing on climate system efficiency, battery capacity retention, and control bus reliability.

## Overview and Baselines

Performance counters accumulate per cabinet over a 15-minute Reporting Period (ROP). Baselines should be established over a full seasonal cycle where possible to account for legitimate environmental differences (e.g., comparing February or August duty cycles against the corresponding season from the previous year).

* **Climate Duty Cycle:** An upward trend at a constant ambient temperature serves as an early warning for filter clogging or fan bearing wear. Maintenance should be scheduled when the duty cycle rises 15–20% above the seasonal baseline, prior to any temperature alarms.
* **Battery Health:** A measurement below 80% of nominal capacity serves as the standard replacement trigger.
* **SCU Availability:** Must remain effectively at 100%. Recurring communication losses indicate control-bus cabling issues, which require prompt resolution to avoid coordination failures during mains power outages (which would prevent coordinated load-shedding).

## Key Performance Indicators (KPIs)

The following table details the key performance indicators used to monitor multicabinet site support hardware:

| KPI | Formula | Description |
| :--- | :--- | :--- |
| **Climate Duty Cycle (%)** | $\frac{\text{ctrClimateActiveTime}}{900} \times 100$ | Share of ROP the climate system ran (%) |
| **Temperature Compliance (%)** | $(1 - \frac{\text{ctrTempExcursionTime}}{900}) \times 100$ | Time within setpoint band (%) |
| **Battery Health (%)** | $\frac{\text{ctrBatteryTestCapacity}}{\text{nominal capacity}} \times 100$ | Measured vs nominal capacity at last test (%) |
| **SCU Availability (%)** | $(1 - \frac{\text{ctrScuCommLossTime}}{900}) \times 100$ | Control-bus availability per cabinet (%) |

## Performance Counters

Counters are collected on a per-cabinet basis. Special attention should be paid to the following:
* **`ctrTempExcursionTime` in expansion cabinets:** Expansion cabinets frequently house newer, denser hardware while relying on the site's older climate design assumptions.
* **`ctrLoadShedEvents` correlation:** Events should correlate exactly with mains-outage events from the power system log. Any uncorrelated load-shed event indicates a rectifier or distribution fault masquerading as an outage.

### Counter Definitions

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
| `ctrClimateActiveTime` | Climate system active time per ROP | 0–900 s | int64 |
| `ctrTempExcursionTime` | Time outside setpoint band per ROP | 0–900 s | int64 |
| `ctrTempMax` | Maximum cabinet temperature per ROP | −40–100 (°C ×10) | int64 |
| `ctrScuCommLossTime` | Control-bus loss time per ROP | 0–900 s | int64 |
| `ctrBatteryTestCapacity` | Capacity from last battery test (Ah ×10) | 0–2³¹ | int64 |
| `ctrLoadShedEvents` | Load-shed sequence executions | 0–2³¹ | int64 |
| `ctrDoorOpenEvents` | Door-open events per cabinet | 0–2³¹ | int64 |

# Cross-References

* [FEATURE OVERVIEW](feature-overview.md)
* [FEATURE OPERATION](feature-operation.md)
* [PARAMETERS](parameters.md)
