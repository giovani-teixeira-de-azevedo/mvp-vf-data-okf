---
type: concept
resource: data/vodafone-mvp/raw/Energy Metering.pdf#feature-overview
title: Feature Overview
description: Provides standardized, per-unit measurement and reporting of electrical
  energy consumption across radio site equipment managed by a node.
tags:
- energy-metering
- energy-efficiency
- ran
- performance-management
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:43:08+00:00'
  source_sha256: 3cd817414f3a2988
sources:
- title: Energy Metering
  resource: data/vodafone-mvp/raw/Energy Metering.pdf
---

The Energy Metering feature provides standardized, per-unit measurement and reporting of electrical energy consumption across radio site equipment managed by a node. This section outlines the feature's core capabilities, measurement methodology, hardware-dependent accuracy, and its role as a foundation for energy efficiency functions.

## Core Capabilities

Energy Metering enables per-unit measurement of the electrical energy consumed by:
* **Baseband units**
* **Radio units** (including Active Antenna System (AAS) radios)
* **Enclosure infrastructure**
* **Site-level equipment** (where supported power distribution hardware is present)

This functionality positions the RAN node as a primary data source for operator sustainability initiatives, energy cost allocation, and standardized energy-efficiency KPIs defined in **ETSI ES 202 336** and the 3GPP energy efficiency framework (**TS 28.310**).

## Measurement and Performance Management Integration

Modern radio hardware integrates power measurement circuitry at the DC feed of each unit, sampling voltage and current at sub-second resolution. 

* **Standard Operation:** Without this feature active, power data is only accessible as instantaneous diagnostics readings.
* **Integration:** Energy Metering integrates these measurements into the [Performance Management](performance-management.md) framework.
* **Granularity:** Energy in watt-hours (Wh) is accumulated per unit and per 15-minute Recording Observation Period (ROP), then aggregated at the node level.
* **Exposure:** Data is exposed via PM counters (such as `ctrEnergyConsumed` per unit) and on-demand Configuration Management (CM) attributes (for momentary power and cumulative energy).

### Measurement Accuracy

Accuracy varies depending on hardware capabilities:
* **Integrated Metering (typically ±2%):** For radio units containing built-in measurement circuitry.
* **Model-Based Estimation (typically ±5%):** For older hardware lacking measurement circuitry, where consumption is calculated using a pre-characterized power model.

With this feature enabled, typical site-level visibility covers **95%+** of total site RAN consumption (metered or estimated), with the remaining portion consisting of unmanaged auxiliary equipment.

## Foundation for Energy-Efficiency Features

This feature is foundational to verifying energy savings across the node. The savings claimed by other functions can only be validated against the baseline and active measurements provided by Energy Metering. These functions include:
* NR Massive MIMO Sleep Mode
* NR Booster Carrier Sleep
* Radio Deep Sleep Mode
* NR Dynamic Power Optimizer

Additionally, network-level KPIs such as bits-per-joule rely on these measurements for a trustworthy denominator. The measured data also helps recalibrate model-based savings estimates within sleep features, improving the accuracy of node-level savings dashboards.

## Functional Architecture

The flow from physical measurements to reporting systems is illustrated in the diagram below:

```mermaid
flowchart TB 
    subgraph Site 
        BB[Baseband Unit<br/>measured at DC feed] 
        RU1[Radio Unit 1<br/>integrated metering] 
        RU2[Radio Unit 2<br/>model-based estimate] 
        PSU[Power Distribution Unit<br/>site-level metering] 
    end 
    BB --> AGG[Energy Metering function<br/>per-unit accumulation, per ROP] 
    RU1 --> AGG 
    RU2 --> AGG 
    PSU --> AGG 
    AGG --> PM[PM counters<br/>ctrEnergyConsumed per unit] 
    AGG --> CM[On-demand attributes<br/>momentary power, cumulative energy] 
    PM --> OSS[OSS / Energy reporting<br/>bits-per-joule, sustainability KPIs]
```

# Cross-References

* [Performance Management](performance-management.md) — For information on energy-related PM counters and attributes.
* [Feature Dependencies](feature-depedencies.md) — For dependencies and relationships with energy-saving features.
* [Feature Operation](feature-operation.md) — For operational characteristics and configuration.
* [Activation Procedure](activation-procedure.md) — For instructions on enabling this feature.
