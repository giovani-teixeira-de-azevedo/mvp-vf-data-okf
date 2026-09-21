---
type: concept
resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf#feature-depedencies
title: Feature Dependencies and Limitations
description: Details the feature, hardware, and network dependencies, as well as operational
  limitations, for Dynamic Component Carrier Management.
tags:
- dependencies
- limitations
- carrier-aggregation
- licensing
- hardware-requirements
- network-requirements
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:38:09+00:00'
  source_sha256: 713e3679c3ec7aaf
sources:
- resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf
  title: Dynamic Component Carrier Management
---

Dynamic Component Carrier Management sits between baseline carrier aggregation (CA) features and higher-level policy features. The ordering of decision authority is crucial for proper functioning and interworking.

## Feature Dependencies

* **NR Downlink Carrier Aggregation**: Requires NR DL Carrier Aggregation to be activated. Uplink carriers are managed only if NR Uplink Carrier Aggregation is also active.
* **Licensing and Control**: Requires a valid license key (`FAK-33013`) and the parameter `FeatureCtrl=DynamicCcMgmt` set to `ACTIVATED`.
* **Data-Aware Carrier Management**: Interworks with this feature, where demand classification decides whether SCells are added, and Dynamic Component Carrier Management decides which carriers are chosen.
* **User- and Service-Specific Carrier Aggregation**: Policies from User- and Service-Specific Carrier Aggregation override dynamic scoring for matching subscriber groups.
* **Automated Carrier Aggregation (O&M category)**: Automated candidate discovery can populate the initial carrier relations consumed by this feature.

## Hardware Dependencies

* **Radio Hardware**: No specific radio hardware requirements; all carriers visible to the node's baseband can be managed.
* **Multi-Baseband Sites**: Cross-baseband carrier management requires the inter-baseband coordination link.

## Network Dependencies

* **Carrier Location**: Manages only node-internal carriers by default. Managing carriers on other nodes requires NR Carrier Aggregation Scheduling Optimization for External SCells.
* **Core Network**: No core network dependencies.

## Limitations

* **Signaling Capacity**: Rebalancing migrations are capped at `maxMigrationsPerRop` per carrier to protect RRC signaling capacity.
* **Score Re-evaluation**: Score re-evaluation for a connected UE occurs at most every 10 seconds. Faster load transients are handled by the scheduler rather than carrier reassignment.
* **EMF Power Lock**: Carriers in EMF power lock (see EMF Power Lock Mid-Band) are scored but capped at their locked capacity.
* **Candidate Evaluation**: A maximum of 8 candidate carriers can be evaluated per UE.

# Cross-References

* [Feature Overview](feature-overview.md)
* [Feature Operation](feature-operation.md)
* [Parameters](parameters.md)
* [Activation Procedure](activation-procedure.md)
