---
type: concept
resource: data/vodafone-mvp/raw/Data-Aware Carrier Management.pdf#feature-depedencies
title: Feature Dependencies
description: Outlines the software, hardware, and network dependencies, interworking
  capabilities, and technical limitations of the Data-Aware Carrier Management feature.
tags:
- carrier-aggregation
- dependencies
- licensing
- limitations
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:37:35+00:00'
  source_sha256: 3d0ad4d8656e68d8
sources:
- resource: data/vodafone-mvp/raw/Data-Aware Carrier Management.pdf
  title: Data-Aware Carrier Management
---

The **Data-Aware Carrier Management** feature operates as a decision layer on top of the baseline carrier aggregation (CA) machinery, rather than implementing CA itself. The software, hardware, and network dependencies, as well as feature interworking and limitations, are detailed below.

## Feature Dependencies

* **NR Carrier Aggregation**: Requires NR Downlink Carrier Aggregation to be activated. If uplink secondary cells (SCells) are managed, NR Uplink Carrier Aggregation must also be active.
* **Licensing & Parameters**: Requires a valid license key (`FAK-33012`) and the parameter `FeatureCtrl=DataAwareCarrierMgmt` set to `ACTIVATED`.
* **NR Intelligent SCell Management Interworking**: When both are active, Data-Aware Carrier Management governs *whether* SCells are configured or activated, while Intelligent SCell Management governs *which* candidate carriers are ranked highest.
* **User- and Service-Specific Carrier Aggregation Interworking**: Subscriber-group policies take precedence over demand classification.
* **NR QoS-Aware Downlink Carrier Aggregation**: The demand classes are exposed to this feature when both features are active.

## Hardware Dependencies

* **Radio Hardware**: No radio hardware dependencies; the feature runs entirely in the baseband.
* **Baseband Capacity**: Baseband units must have capacity headroom for the per-UE estimator (approximately 1% additional load at 1,000 connected UEs).

## Network Dependencies

* **Core & Transport**: No core or transport network dependencies. Demand estimation is entirely RAN-internal and does not require NWDAF (Network Data Analytics Function) or core analytics.
* **EN-DC Deployments**: In E-UTRA-NR Dual Connectivity (EN-DC) deployments, the feature manages NR SCells only; LTE SCell management is unaffected.

## Limitations

* **Demand History Persistence**: Demand history is kept only for the duration of the RRC connection plus the RRC Inactive suspension; it is not persisted across registrations.
* **Demand Classes**: A maximum of 4 demand classes is supported in this release. The class definitions are fixed; only the thresholds are configurable.
* **Voice Bearers**: UEs with active `5QI-1` (voice) bearers are never classified as `BACKGROUND` to protect call setup responsiveness.
* **Frequency Range**: Not applicable to FR2-only UEs, where SCell setup is governed by beam management timing.

# Cross-References

* [Feature Overview](feature-overview.md) — For an overview of the core functionality of the feature.
* [Feature Operation](feature-operation.md) — For details on the demand classification mechanism.
* [Parameters](parameters.md) — For details on the configuration parameters, including the feature control switch and class thresholds.
* [Activation Procedure](activation-procedure.md) — For the step-by-step activation of the feature and license key verification.
