---
type: concept
resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf#feature-depedencies
title: Feature Dependencies
description: Defines the feature, hardware, network dependencies, and operational
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
  at: '2026-09-24T08:37:01+00:00'
  source_sha256: 78c4ba22ad2e9034
sources:
- title: Automated Energy Saver
  resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf
---

This section details the feature, hardware, and network dependencies, as well as operational limitations, for the Automated Energy Saver feature. The feature operates as an orchestrator, delivering no savings by itself and requiring at least one subordinate energy feature to be licensed and activated.

## Feature Dependencies

- Requires a valid license key (`FAK-31510`) installed and `FeatureCtrl=AutomatedEnergySaver` set to `ACTIVATED`.
- Requires at least one of the following features to be licensed and activated on the node:
  - NR Booster Carrier Sleep
  - NR Massive MIMO Sleep Mode
  - Radio Deep Sleep Mode
  - NR Micro Sleep Tx
  - Energy-Optimized Slot Allocation
- When active, it takes ownership of the enter/exit thresholds and time-of-day windows of the subordinate features; manually configured values on those features are ignored while `energySaverMode=AUTO`.
- Interworks with Energy Metering for measured (rather than model-based) savings reporting.

## Hardware Dependencies

- No dedicated hardware required. Runs on the baseband unit's management plane processor.
- The reachable saving depth per radio depends on the capabilities of the subordinate features on that radio (for example, per-branch power gating requires AAS radio generation R2 or later).

## Network Dependencies

- Requires PM counter collection to have been active on the node for at least 7 days before predictions become effective; until then, the feature operates in a conservative fallback mode using instantaneous load only.
- In Multi-Operator RAN deployments, the prediction model uses aggregated load across all sharing operators.

## Limitations

- Maximum of 24 cells per node under automated control.
- The prediction model does not anticipate unscheduled events (concerts, incidents); the reactive exit mechanisms of the subordinate features remain the safety net, so a mis-prediction costs saving opportunity, never availability.
- Cells carrying an active PWS (Public Warning System) broadcast are excluded from automated actions for the duration of the broadcast.

# Cross-References

- [Feature Overview](feature-overview.md)
- [Parameters](parameters.md)
- [Performance Management](performance-management.md)
- [Activation Procedure](activation-procedure.md)
