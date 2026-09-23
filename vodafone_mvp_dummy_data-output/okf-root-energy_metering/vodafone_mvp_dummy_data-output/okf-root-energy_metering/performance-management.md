---
type: concept
resource: data/vodafone-mvp/raw/Energy Metering.pdf#performance-management
title: PERFORMANCE MANAGEMENT
description: Defines the monitoring strategy, key performance indicators (KPIs), and
  performance counters for node energy consumption and efficiency.
tags:
- energy-metering
- performance-management
- kpi
- counters
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T10:26:34+00:00'
  source_sha256: c22cde8838d50f12
sources:
- resource: data/vodafone-mvp/raw/Energy Metering.pdf
  title: Energy Metering
---

This section details the performance management strategy, key performance indicators (KPIs), and measurement counters for monitoring energy consumption and efficiency across network nodes.

## Monitoring Strategy

Performance management is the primary output of the energy metering feature rather than a supervision layer on top of it. The monitoring strategy consists of two main strands:

- **Absolute Consumption Trending:** Trend the absolute energy consumption per unit and per node against a baseline captured over at least one full week, accounting for strong daily and weekly traffic-driven consumption profiles.
- **Efficiency Ratio Trending:** Trend the efficiency ratio (traffic volume against energy) to normalize out load variations.

All counters accumulate per 15-minute Result Output Period (ROP). Analyses should compare like hours with like hours (e.g., comparing a Tuesday busy hour with a Sunday night reflects traffic variation rather than operational efficiency).

## Key Performance Indicators

Node Energy Efficiency (bits per joule) serves as the headline KPI. Expected values range from 50–200 kbit/J on loaded mid-band macro sites, and an order of magnitude lower on lightly loaded coverage sites. Absolute values are site-specific, meaning actions should be driven by trends rather than absolute levels.

Key operational guidelines for KPIs:
- A step change in Average Node Power without a matching traffic change indicates either an activated or deactivated energy feature (which should be verified against configuration change logs) or a hardware fault such as a failed sleep function.
- A Metering Coverage level below 90% indicates that estimated values dominate; caution should be exercised when using data from such nodes for financial reporting.

| KPI | Formula | Description |
| --- | --- | --- |
| Average Node Power | `ctrNodeEnergyConsumed × 4 / 1000` | Mean node power over the ROP (kW; Wh×4 = W) |
| Node Energy Efficiency | `ctrDataVolume / (ctrNodeEnergyConsumed × 3600)` | Delivered bits per joule |
| Metering Coverage | `ctrEnergyMeasured / ctrNodeEnergyConsumed × 100` | Share of reported energy that is measured, not estimated (%) |
| Radio Share of Consumption | `ctrRadioEnergyConsumed / ctrNodeEnergyConsumed × 100` | Radio units' share of node energy (%) |

## Performance Counters

Counters split energy measurements by unit type and measurement method:
- `ctrEnergyEstimated` requires attention during hardware-refresh programs and should trend toward zero as legacy units are replaced.
- A sudden rise in `ctrRadioEnergyConsumed` on one radio unit while its sibling radios remain flat serves as an early indicator of a hardware issue (such as fan degradation or power amplifier bias drift) and typically precedes a temperature alarm by weeks.

| Counter | Description | Range | Datatype |
| --- | --- | --- | --- |
| `ctrNodeEnergyConsumed` | Total node energy per ROP (Wh) | 0–2³¹ | int64 |
| `ctrRadioEnergyConsumed` | Sum of radio unit energy per ROP (Wh) | 0–2³¹ | int64 |
| `ctrBasebandEnergyConsumed` | Baseband unit energy per ROP (Wh) | 0–2³¹ | int64 |
| `ctrEnergyMeasured` | Portion of node energy from hardware meters (Wh) | 0–2³¹ | int64 |
| `ctrEnergyEstimated` | Portion of node energy from power models (Wh) | 0–2³¹ | int64 |
| `ctrSitePowerDelta` | Distribution/rectifier losses from site power system (Wh) | 0–2³¹ | int64 |
| `ctrDataVolume` | Total delivered data volume per ROP (bits) | 0–2⁶³ | int64 |
