---
type: concept
resource: data/vodafone-mvp/raw/Energy Metering.pdf#feature-operation
title: Feature Operation
description: Explains how the Energy Metering function samples, accumulates, and estimates
  energy consumption across managed units.
tags:
- energy-metering
- feature-operation
- power-estimation
- pm-counters
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:43:12+00:00'
  source_sha256: 20dac035d91777de
sources:
- resource: data/vodafone-mvp/raw/Energy Metering.pdf
  title: Energy Metering
---

The Energy Metering function tracks and reports power consumption across different physical components within a network node. This section describes the measurement and estimation mechanisms, aggregation rules, and reporting behaviors of the feature.

## Measurement and Estimation Mechanisms

The feature handles energy tracking through two distinct methods, depending on the capabilities of the hardware.

### Hardware-Based Measurement

For managed units equipped with dedicated physical metering hardware (such as certain Radio Units or Baseband Units):
* **Sampling:** The unit samples its DC input voltage and current at a rate of 1 Hz or faster.
* **Integration:** The sampled voltage and current values are integrated to determine energy consumption.
* **Polling:** The Energy Metering function polls each metered unit once per second over the internal equipment bus to accumulate energy.
* **PM Snapshots:** At Recording Output Period (ROP) boundaries, the accumulated energy is snapshotted into Performance Management (PM) counters.
* **Direct Monitoring:** Momentary power and lifetime cumulative energy are exposed as readable attributes on each `RadioUnit`, `BasebandUnit`, and power-related equipment Managed Object (MO). These values support manual spot checks for troubleshooting and energy audits.

### Model-Based Estimation

For units lacking physical measurement hardware, the function computes a software-based estimate using the unit's characterized power model. The estimation is calculated from:
* A base power consumption.
* Load-dependent terms, which are dynamically driven by:
  * Transmitted power
  * Active branch count
  * Carrier configuration

The underlying estimation model is identical to the one used by sleep-mode features to compute `ctrEnergySavedEstimate`-type counters, ensuring methodological consistency between measured and estimated performance figures.

## Node-Level Aggregation and Counter Reset Behavior

To provide an overall node-level energy consumption view, the function aggregates individual unit metrics:

* **Node-Level Aggregate:** The aggregate is computed as the sum of all per-unit values (both measured and estimated). When present, it also includes the delta measured by the site power system to capture distribution losses.
* **ROP Counters:** PM counters accumulate monotonically within each ROP and are reset at ROP boundaries.
* **Lifetime Cumulative Attributes:** The lifetime cumulative attributes exposed on the MOs never reset, except when the physical hardware is replaced.

# Cross-References

* [Feature Overview](feature-overview.md) — For a high-level overview of the Energy Metering capabilities.
* [Performance Management](performance-management.md) — For details on the PM counters and ROP snapshots populated by this feature.
* [Parameters](parameters.md) — For configuration parameters related to the energy metering and estimation models.
