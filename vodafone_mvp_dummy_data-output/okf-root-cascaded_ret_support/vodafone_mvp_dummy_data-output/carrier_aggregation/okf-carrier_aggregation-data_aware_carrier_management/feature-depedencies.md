---
type: concept
resource: data/vodafone-mvp/raw/Data-Aware Carrier Management.pdf#feature-depedencies
title: Feature Dependencies
description: Outlines software, hardware, network dependencies, interworking relationships,
  and limitations for Data-Aware Carrier Management.
tags:
- carrier-aggregation
- dependencies
- hardware-requirements
- limitations
- licensing
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T13:48:19+00:00'
  source_sha256: 3d0ad4d8656e68d8
sources:
- title: Data-Aware Carrier Management
  resource: data/vodafone-mvp/raw/Data-Aware Carrier Management.pdf
---

This section details the feature, hardware, and network dependencies, interworking relationships, and technical limitations of Data-Aware Carrier Management. The feature operates as a decision layer on top of baseline carrier aggregation machinery and does not itself implement carrier aggregation.

## Feature Dependencies

- **Carrier Aggregation Baseline:** Requires NR DL Carrier Aggregation (and NR Uplink Carrier Aggregation if uplink SCells are managed) to be activated.
- **Licensing and Control Parameter:** Requires a valid license key (`FAK-33012`) and `FeatureCtrl=DataAwareCarrierMgmt` set to `ACTIVATED`.
- **NR Intelligent SCell Management Interworking:** When both features are active, Data-Aware Carrier Management governs whether SCells are configured/activated, while Intelligent SCell Management governs which candidate carriers are ranked highest.
- **User- and Service-Specific Carrier Aggregation Interworking:** Subscriber-group policies take precedence over demand classification.
- **NR QoS-Aware Downlink Carrier Aggregation Interworking:** Demand classes are exposed to NR QoS-Aware Downlink Carrier Aggregation when both features are active.

## Hardware Dependencies

- **Radio Hardware:** No radio hardware dependencies; the feature runs entirely in baseband.
- **Baseband Load:** Baseband units must have capacity headroom for the per-UE estimator (approximately 1% additional load at 1000 connected UEs).

## Network Dependencies

- **Core/Transport:** No core or transport dependencies. Demand estimation is RAN-internal and does not require NWDAF or core analytics.
- **EN-DC Deployments:** In EN-DC deployments, the feature manages NR SCells only; LTE SCell management is unaffected.

## Limitations

- **History Retention:** Demand history is kept only for the duration of the RRC connection plus the RRC Inactive suspension; it is not persisted across registrations.
- **Demand Class Limits:** Maximum of 4 demand classes in this release; class definitions are fixed, only thresholds are configurable.
- **Voice Protection:** UEs with active 5QI-1 (voice) bearers are never classified as `BACKGROUND` to protect call setup responsiveness.
- **Frequency Range Scope:** Not applicable to FR2-only UEs where SCell setup is governed by beam management timing.

# Cross-References

- [Feature Overview](feature-overview.md)
- [Feature Operation](feature-operation.md)
- [Parameters](parameters.md)
- [Activation Procedure](activation-procedure.md)
