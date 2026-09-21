---
type: concept
resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf#feature-depedencies
title: Feature Dependencies
description: Details the feature, hardware, and network dependencies, as well as operational
  limitations of the CQI-Based UE Energy Efficiency Enhancement feature.
tags:
- dependencies
- limitations
- licensing
- c-drx
- scheduling
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:38:44+00:00'
  source_sha256: e62699b05496672b
sources:
- resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf
  title: CQI-Based UE Energy Efﬁciency Enhancement
---

This section outlines the feature, hardware, and network dependencies, along with the operational limitations of the CQI-Based UE Energy Efficiency Enhancement feature. The feature modifies scheduler behavior and CSI reporting configuration, and its dependencies concentrate primarily on scheduling and Discontinuous Reception (DRX) features.

It is recommended to review this list per cell before activation, particularly regarding the interaction with latency-sensitive scheduling features.

## Feature Dependencies

* **Licensing and Activation:** Requires a valid license key (`FAK-31525`) installed and the parameter `FeatureCtrl=CqiUeEnergyEff` set to `ACTIVATED` (see [Activation Procedure](activation-procedure.md)).
* **Connected Mode DRX (C-DRX):** Requires Connected Mode DRX to be activated on the cell; the battery gain is realized as additional C-DRX sleep time.
* **NR Service-Adaptive DRX Interworking:** The feature interworks with NR Service-Adaptive DRX. Service-specific DRX profiles are respected, and scheduling compaction operates within the active time they define.
* **Latency-Prioritized Scheduling:** The conservative Modulation and Coding Scheme (MCS) bias is suppressed for bearers handled by Latency-Prioritized Scheduling to avoid adding retransmission-avoidance delay margin to latency-critical traffic.

## Hardware Dependencies

* **No Dedicated Hardware:** The feature runs in the baseband scheduler on all supported baseband units and does not require dedicated hardware.

## Network Dependencies

* **None:** The feature is cell-local and requires no core, transport, or neighbor cell coordination.

## Limitations

* **UE Support:** The relaxed Channel State Information (CSI) reporting interval is applied only to UEs indicating support for the configured CSI report periodicities. Legacy UEs keep their existing configuration (see [Feature Operation](feature-operation.md)).
* **MU-MIMO Compatibility:** Scheduling compaction is not applied to UEs in Multi-User MIMO (MU-MIMO) pairing candidacy, since compact wideband allocations reduce pairing opportunities.
* **Gain Measurement:** Gains are model-estimated on the network side. Actual UE battery savings depend on individual UE implementation and cannot be directly measured by the gNodeB.
* **RedCap UEs:** The feature is not applied to RedCap UEs handled by the Ericsson Reduced Capability Enabler, which utilize their own reduced monitoring framework.

# Cross-References

* [Feature Operation](feature-operation.md) — Details on CSI reporting configuration and scheduling compaction behavior.
* [Activation Procedure](activation-procedure.md) — Instructions on installing the license key and activating the feature control parameter.
