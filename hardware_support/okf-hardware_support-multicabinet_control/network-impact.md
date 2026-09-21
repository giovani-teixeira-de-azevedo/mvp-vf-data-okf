---
type: concept
resource: data/vodafone-mvp/raw/Multicabinet Control.pdf#network-impact
title: Network Impact
description: Explains the impact of the Multicabinet Control feature on traffic, fault
  management, site operations, and energy efficiency.
tags:
- Multicabinet Control
- Network Impact
- Load Shedding
- Alarm Management
- Energy Efficiency
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:45:39+00:00'
  source_sha256: 08f1d0546f186770
sources:
- title: Multicabinet Control
  resource: data/vodafone-mvp/raw/Multicabinet Control.pdf
---

This section details the impact of the Multicabinet Control feature on network traffic, fault management, site operations, and energy consumption.

## Key Impacts

### Traffic and Users
* **Normal Operation**: There is no impact on traffic or users during normal operation.
* **Mains Outages**: During mains outages, a coordinated load-shed order determines which radio functions survive longest on battery backup. This represents a deliberate, configurable service impact.

### Fault Management
* **Unified Pipeline**: All site hardware alarms are consolidated into a single pipeline with cabinet-scoped sources.
* **Supervision Coverage**: Typical site deployments achieve 100% supervision of auxiliary equipment that was previously unsupervised.

### Site Visits
* **Reduction**: The need for physical site visits is reduced.
* **Remote Operations**: Battery tests, climate diagnostics, and sensor checks can be executed remotely across all cabinets.

### Energy Efficiency
* **Auxiliary Energy Savings**: Coordinated climate control typically reduces site auxiliary energy consumption by 5% to 10%.
* **Setpoint Optimization**: These savings are achieved by eliminating counteracting setpoints across different cabinets.

# Cross-References

* [Feature Overview](feature-overview.md) — For general details on the Multicabinet Control feature.
* [Feature Operation](feature-operation.md) — For how the system operates under various scenarios.
* [Parameters](parameters.md) — For configuration parameters related to load-shedding and climate control.
* [Performance Management](performance-management.md) — For monitoring energy savings and site diagnostics.
