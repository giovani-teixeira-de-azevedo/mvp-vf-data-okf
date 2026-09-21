---
type: concept
resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf#feature-depedencies
title: Feature Dependencies
description: Outlines the software, hardware, network dependencies, and limitations
  of the Automated Energy Saver feature.
tags:
- dependencies
- licensing
- limitations
- automated-energy-saver
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:38:02+00:00'
  source_sha256: 78c4ba22ad2e9034
sources:
- resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf
  title: Automated Energy Saver
---

The **Automated Energy Saver** feature acts as an orchestrator and delivers no energy savings by itself. It requires at least one subordinate energy-saving feature to be licensed and activated on the node. The value of this feature scales with the number of subordinate features it can control, so network planning should review dependencies per node during rollout planning.

## Feature Dependencies

- **License and Activation**:
  - Requires a valid license key **FAK-31510** installed.
  - Requires the parameter `FeatureCtrl=AutomatedEnergySaver` to be set to `ACTIVATED` (see [Activation Procedure](activation-procedure.md)).
- **Subordinate Features**: Requires at least one of the following features to be licensed and activated on the node:
  - NR Booster Carrier Sleep
  - NR Massive MIMO Sleep Mode
  - Radio Deep Sleep Mode
  - NR Micro Sleep Tx
  - Energy-Optimized Slot Allocation
- **Control Ownership**: When active, the feature takes ownership of the enter/exit thresholds and time-of-day windows of the subordinate features. Manually configured values on those subordinate features are ignored while the parameter `energySaverMode` is set to `AUTO` (see [Parameters](parameters.md)).
- **Interworking**: Interworks with Energy Metering for measured (rather than model-based) savings reporting.

## Hardware Dependencies

- **Processor**: No dedicated hardware is required. The feature runs on the baseband unit's management plane processor.
- **Radio Capabilities**: The reachable saving depth per radio depends on the capabilities of the subordinate features on that radio (for example, per-branch power gating requires AAS radio generation R2 or later).

## Network Dependencies

- **Performance Monitoring (PM) Collection**: Requires PM counter collection to have been active on the node for at least 7 days before predictions become effective. Until this 7-day period is completed, the feature operates in a conservative fallback mode using instantaneous load only.
- **Multi-Operator RAN (MORAN)**: In MORAN deployments, the prediction model uses the aggregated load across all sharing operators.

## Limitations

- **Cell Limit**: Maximum of 24 cells per node can be under automated control.
- **Unscheduled Events**: The prediction model does not anticipate unscheduled events (such as concerts or incidents). The reactive exit mechanisms of the subordinate features remain active as a safety net. Consequently, a prediction error costs only a saving opportunity, never network availability.
- **Public Warning System (PWS)**: Cells carrying an active PWS broadcast are excluded from automated actions for the duration of the broadcast.

# Cross-References

- [Feature Overview](feature-overview.md)
- [Parameters](parameters.md)
- [Activation Procedure](activation-procedure.md)
