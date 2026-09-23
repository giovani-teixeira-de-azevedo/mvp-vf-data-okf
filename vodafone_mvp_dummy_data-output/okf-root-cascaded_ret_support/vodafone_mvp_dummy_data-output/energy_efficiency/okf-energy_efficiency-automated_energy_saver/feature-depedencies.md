---
type: concept
resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf#feature-depedencies
title: Feature Dependencies
description: Details the feature, hardware, and network dependencies along with operational
  limitations for Automated Energy Saver.
tags:
- automated-energy-saver
- dependencies
- hardware-dependencies
- network-dependencies
- limitations
- licensing
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T14:00:58+00:00'
  source_sha256: 78c4ba22ad2e9034
sources:
- title: Automated Energy Saver
  resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf
---

This section details the feature, hardware, and network dependencies as well as operational limitations for the Automated Energy Saver feature.

The Automated Energy Saver feature acts as an orchestrator that delivers no energy savings by itself and requires at least one subordinate energy feature to be licensed and activated. The value of the feature scales with the number of subordinate features it can control, so reviewing the dependency list per node during rollout planning is necessary.

## Feature Dependencies

- **Licensing and Activation:** Requires a valid license key (`FAK-31510`) installed and `FeatureCtrl=AutomatedEnergySaver` set to `ACTIVATED`.
- **Subordinate Features:** Requires at least one of the following features to be licensed and activated on the node:
  - NR Booster Carrier Sleep
  - NR Massive MIMO Sleep Mode
  - Radio Deep Sleep Mode
  - NR Micro Sleep Tx
  - Energy-Optimized Slot Allocation
- **Control Ownership:** When active, the feature takes ownership of the enter/exit thresholds and time-of-day windows of the subordinate features. Manually configured values on those subordinate features are ignored while `energySaverMode=AUTO`.
- **Energy Metering Interworking:** Interworks with Energy Metering for measured (rather than model-based) savings reporting.

## Hardware Dependencies

- **Processor Requirements:** Requires no dedicated hardware and runs on the baseband unit's management plane processor.
- **Radio Capabilities:** The reachable saving depth per radio depends on the capabilities of the subordinate features on that radio (for example, per-branch power gating requires AAS radio generation R2 or later).

## Network Dependencies

- **PM Counter Collection:** Requires PM counter collection to have been active on the node for at least 7 days before predictions become effective. Until that time, the feature operates in a conservative fallback mode using instantaneous load only.
- **Multi-Operator RAN (MOCN):** In Multi-Operator RAN deployments, the prediction model uses aggregated load across all sharing operators.

## Limitations

- **Cell Capacity Limit:** Maximum of 24 cells per node under automated control.
- **Unscheduled Events:** The prediction model does not anticipate unscheduled events (such as concerts or incidents). The reactive exit mechanisms of the subordinate features remain the safety net; a mis-prediction impacts saving opportunity but never availability.
- **Public Warning System (PWS):** Cells carrying an active PWS broadcast are excluded from automated actions for the duration of the broadcast.

# Cross-References

- [Feature Overview](feature-overview.md)
- [Feature Operation](feature-operation.md)
- [Parameters](parameters.md)
- [Performance Management](performance-management.md)
