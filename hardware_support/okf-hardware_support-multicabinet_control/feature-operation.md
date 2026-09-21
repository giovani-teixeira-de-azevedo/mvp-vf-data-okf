---
type: concept
resource: data/vodafone-mvp/raw/Multicabinet Control.pdf#feature-operation
title: Feature Operation
description: Describes the operational behavior, synchronization, climate coordination,
  and safety mechanisms of the Multicabinet Control feature.
tags:
- Multicabinet Control
- SCU
- Site Control Bus
- Climate Coordination
- Load-Shed Sequence
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:45:42+00:00'
  source_sha256: afb08731d1f0c963
sources:
- resource: data/vodafone-mvp/raw/Multicabinet Control.pdf
  title: Multicabinet Control
---

This section describes the operational lifecycle and runtime mechanisms of the Multicabinet Control feature, including cabinet discovery, binding, data synchronization, climate coordination, and autonomous safety protections.

## Discovery and Binding

At activation, the primary Site Control Unit (SCU) scans the site control bus to discover secondary SCUs. 
* **Binding Process:** Each discovered secondary unit is bound to a `Cabinet MO` created by the operator.
* **Security & Isolation:** Explicit binding by serial number is used to prevent accidental adoption of a neighboring operator's cabinet on shared sites.

## Configuration Synchronization and Monitoring

Once binding is complete, the primary SCU synchronizes the configuration to each secondary unit. The synchronized parameters include:
* Climate setpoints
* Alarm thresholds
* Battery test schedules
* Load-shed priorities

### Runtime Monitoring
After configuration synchronization, the secondary SCUs continuously stream sensor data and events to the primary SCU at a frequency of **1 Hz**. The primary SCU consolidates this data into the node's equipment model.

## Active Runtime Functions

### Climate Coordination
Climate coordination is the most active runtime function. The primary SCU balances setpoints across cabinets to prevent thermal oscillation (e.g., preventing one cabinet from cooling while an adjacent cabinet sharing a wall or airflow path heats).

### Mains Outage and Load-Shedding
During a mains power outage, the primary SCU coordinates and executes a site-wide load-shed sequence:
1. **Load Shedding Order:** Non-critical loads are shed in the declared order.
2. **Timing Control:** The timing of the sequence is steered by the per-cabinet battery state of charge (SoC).

## Local Safety Protections
All local safety protections remain fully autonomous within each individual SCU. These include:
* Over-temperature shutdown
* Smoke response

While coordination via the primary SCU adds operational intelligence, the autonomous design ensures that the coordination link never becomes a single point of failure for safety-critical functions.

# Cross-References

* [Feature Overview](feature-overview.md) — For a high-level overview of the Multicabinet Control feature.
* [Activation Procedure](activation-procedure.md) — For details on activating the feature and initial setup.
* [Parameters](parameters.md) — For details on configuration and operational parameters.
