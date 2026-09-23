---
type: concept
resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf#feature-depedencies
title: Feature Dependencies
description: Outlines feature, hardware, and network dependencies along with operational
  limitations for CQI-Based UE Energy Efficiency Enhancement.
tags:
- dependencies
- limitations
- drx
- hardware
- license
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T14:20:37+00:00'
  source_sha256: e62699b05496672b
sources:
- resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf
  title: CQI-Based UE Energy Efﬁciency Enhancement
---

The feature modifies scheduler behavior and CSI reporting configuration, so its dependencies concentrate on scheduling and DRX features. Review the list per cell before activation, particularly the interaction with latency-sensitive scheduling features.

## Feature Dependencies

* Requires a valid license key (FAK-31525) installed and `FeatureCtrl=CqiUeEnergyEff` set to `ACTIVATED`.
* Requires Connected Mode DRX to be activated on the cell; the battery gain is realized as additional C-DRX sleep time.
* Interworks with NR Service-Adaptive DRX: service-specific DRX profiles are respected, and compaction operates within the active time they define.
* The conservative MCS bias is suppressed for bearers handled by Latency-Prioritized Scheduling to avoid adding retransmission-avoidance delay margin to latency-critical traffic.

## Hardware Dependencies

* No dedicated hardware. Runs in the baseband scheduler on all supported baseband units.

## Network Dependencies

* None. The feature is cell-local and requires no core, transport, or neighbor coordination.

## Limitations

* The relaxed CSI reporting interval is applied only to UEs indicating support for the configured CSI report periodicities; legacy UEs keep their existing configuration.
* Scheduling compaction is not applied to UEs in MU-MIMO pairing candidacy, since compact wideband allocations reduce pairing opportunities.
* Gains are model-estimated on the network side; actual UE battery saving depends on UE implementation and cannot be directly measured by the gNodeB.
* Not applied to RedCap UEs handled by Ericsson Reduced Capability Enabler, which have their own reduced monitoring framework.

# Cross-References

* [Feature Overview](feature-overview.md)
* [Feature Operation](feature-operation.md)
* [Parameters](parameters.md)
* [Activation Procedure](activation-procedure.md)
