---
type: concept
resource: data/vodafone-mvp/raw/Energy Metering.pdf#feature-operation
title: FEATURE OPERATION
description: Describes the operational mechanisms of the Energy Metering function,
  including hardware sampling, power estimation models, and node-level energy aggregation.
tags:
- energy-metering
- feature-operation
- pm-counters
- radiounit
- basebandunit
- rop
- power-estimation
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T10:26:30+00:00'
  source_sha256: 20dac035d91777de
sources:
- resource: data/vodafone-mvp/raw/Energy Metering.pdf
  title: Energy Metering
---

This section details the operational mechanics of the Energy Metering feature, including hardware sampling, power estimation models for unmeasured units, and node-level energy aggregation.

## Hardware-Based Energy Metering

Each managed unit equipped with metering capability samples its DC input voltage and current at a rate of 1 Hz or faster and integrates the product to determine energy. The Energy Metering function operates as follows:

* **Polling and Accumulation:** The function polls each unit once per second over the internal equipment bus and accumulates energy per unit.
* **PM Snapshots:** At Result Output Period (ROP) boundaries, the accumulated energy is snapshot into Performance Management (PM) counters.
* **Readable Attributes:** Momentary power and lifetime cumulative energy are exposed as readable attributes on:
  * `RadioUnit`
  * `BasebandUnit`
  * Power-related equipment Managed Objects (MOs)

These readable attributes support spot checks during troubleshooting and energy audits.

## Power Model Estimation

For units without measurement hardware, the function computes an estimate using the unit's characterized power model, which includes:

* Base consumption
* Load-dependent terms driven by:
  * Transmitted power
  * Active branch count
  * Carrier configuration

The estimation model matches the model used by sleep-mode features to compute `ctrEnergySavedEstimate`-type counters, ensuring methodological consistency between measured and estimated figures.

## Node-Level Aggregation and Counter Behavior

The node-level aggregate energy is determined by adding all per-unit values and, when present, the delta measured by the site power system (which captures distribution losses).

Counter and attribute reset behavior is as follows:

* **ROP Counters:** Monotonically accumulate within the ROP and reset at ROP boundaries.
* **Lifetime Cumulative Attributes:** Monotonically accumulate and never reset except upon hardware replacement.

# Cross-References

* [FEATURE OVERVIEW](feature-overview.md)
* [PERFORMANCE MANAGEMENT](performance-management.md)
