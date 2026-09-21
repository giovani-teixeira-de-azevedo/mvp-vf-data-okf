---
type: concept
resource: data/vodafone-mvp/raw/Energy Metering.pdf#performance-management
title: Performance Management
description: Monitoring strategy, key performance indicators (KPIs), and performance
  counters for tracking node energy efficiency and consumption.
tags:
- performance-management
- energy-metering
- kpis
- counters
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:43:21+00:00'
  source_sha256: c22cde8838d50f12
sources:
- title: Energy Metering
  resource: data/vodafone-mvp/raw/Energy Metering.pdf
---

Performance Management is an essential component of the Energy Metering feature, serving as the direct means of service verification and efficiency monitoring. It provides a structured framework of metrics and counters to evaluate energy consumption profiles and system efficiency.

## Monitoring Strategy

The monitoring strategy consists of two main strands:
1. **Trend Absolute Consumption:** Trend the absolute consumption per unit and per node against a baseline captured over at least one full week (as energy consumption has a strong daily and weekly traffic-driven profile).
2. **Trend Efficiency Ratio:** Trend the efficiency ratio (traffic volume against energy) which normalizes out load variations.

### Key Guidelines
* **15-minute ROP:** All performance counters accumulate per 15-minute Recording Observation Period (ROP).
* **Compare Like with Like:** Compare like hours with like hours. Comparing a Tuesday busy hour with a Sunday night reveals traffic differences rather than efficiency changes.

## Key Performance Indicators (KPIs)

* **Node Energy Efficiency (bits per joule)** is the headline KPI. 
  * Expect **50–200 kbit/J** on loaded mid-band macro sites.
  * Expect an order of magnitude lower on lightly loaded coverage sites.
  * Absolute values are site-specific; network operators should focus on trends rather than raw levels.
* **Average Node Power Step Changes:** A step change in Average Node Power without a matching traffic change indicates either:
  * An activated or deactivated energy feature (which should be verified against configuration change logs).
  * A hardware fault, such as a failed sleep function.
* **Metering Coverage Threshold:** Metering Coverage below 90% means that power model estimates dominate. Cautious usage of these nodes is recommended for financial reporting.

### KPI Formulas

| KPI | Formula | Description |
| :--- | :--- | :--- |
| **Average Node Power** | `ctrNodeEnergyConsumed × 4 / 1000` | Mean node power over the ROP (kW; Wh × 4 = W) |
| **Node Energy Efficiency** | `ctrDataVolume / (ctrNodeEnergyConsumed × 3600)` | Delivered bits per joule |
| **Metering Coverage** | `ctrEnergyMeasured / ctrNodeEnergyConsumed × 100` | Share of reported energy that is measured, not estimated (%) |
| **Radio Share of Consumption** | `ctrRadioEnergyConsumed / ctrNodeEnergyConsumed × 100` | Radio units' share of node energy (%) |

## Counters

The performance counters partition energy consumption by unit type and by measurement method (measured vs. estimated):
* **ctrEnergyEstimated:** Deserves close attention during hardware-refresh programs. This counter should trend toward zero as legacy units are replaced with modern hardware.
* **ctrRadioEnergyConsumed:** A sudden rise in this counter on a single radio while its neighboring/sibling units remain flat is a strong early indicator of hardware issues (e.g., fan degradation, PA bias drift). This typically precedes a temperature alarm by weeks.

### Counter Definitions

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
| `ctrNodeEnergyConsumed` | Total node energy per ROP (Wh) | 0–2³¹ | int64 |
| `ctrRadioEnergyConsumed` | Sum of radio unit energy per ROP (Wh) | 0–2³¹ | int64 |
| `ctrBasebandEnergyConsumed` | Baseband unit energy per ROP (Wh) | 0–2³¹ | int64 |
| `ctrEnergyMeasured` | Portion of node energy from hardware meters (Wh) | 0–2³¹ | int64 |
| `ctrEnergyEstimated` | Portion of node energy from power models (Wh) | 0–2³¹ | int64 |
| `ctrSitePowerDelta` | Distribution/rectifier losses from site power system (Wh) | 0–2³¹ | int64 |
| `ctrDataVolume` | Total delivered data volume per ROP (bits) | 0–2⁶³ | int64 |

## Cross-References

* [Feature Overview](feature-overview.md)
* [Parameters](parameters.md)
* [Network Impact](network-impact.md)
* [Feature Operation](feature-operation.md)
* [Activation Procedure](activation-procedure.md)
* [Deactivation Procedure](deactivation-procedure.md)
