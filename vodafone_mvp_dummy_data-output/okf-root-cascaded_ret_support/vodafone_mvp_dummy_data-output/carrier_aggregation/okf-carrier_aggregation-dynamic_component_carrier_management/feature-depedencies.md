---
type: concept
resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf#feature-depedencies
title: Feature Dependencies
description: Outlines feature, hardware, and network dependencies as well as operational
  limitations for Dynamic Component Carrier Management.
tags:
- dynamic-cc-mgmt
- dependencies
- hardware-dependencies
- network-dependencies
- limitations
- carrier-aggregation
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T15:05:05+00:00'
  source_sha256: 713e3679c3ec7aaf
sources:
- resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf
  title: Dynamic Component Carrier Management
---

This section details the feature, hardware, and network dependencies as well as operational limitations for Dynamic Component Carrier Management. Dynamic Component Carrier Management sits between baseline Carrier Aggregation (CA) features and higher-level policy features, where decision authority ordering applies.

## Feature Dependencies

* **NR DL Carrier Aggregation**: Requires NR DL Carrier Aggregation to be activated. Uplink carriers are managed only if NR Uplink Carrier Aggregation is also active.
* **Licensing and Feature Control**: Requires a valid license key (`FAK-33013`) and parameter `FeatureCtrl=DynamicCcMgmt` set to `ACTIVATED`.
* **Data-Aware Carrier Management**: Interworks with Data-Aware Carrier Management; demand classification decides whether SCells are added, while this feature decides which carriers are chosen.
* **User- and Service-Specific Carrier Aggregation**: Policies from User- and Service-Specific Carrier Aggregation override dynamic scoring for matching subscriber groups.
* **Automated Carrier Aggregation**: The automated candidate discovery of Automated Carrier Aggregation (O&M category) can populate the initial carrier relations consumed by this feature.

## Hardware Dependencies

* **Radio Hardware**: No specific radio hardware requirements; all carriers visible to the node's baseband can be managed.
* **Multi-Baseband Sites**: Cross-baseband carrier management requires the inter-baseband coordination link on multi-baseband sites.

## Network Dependencies

* **Carrier Scope**: Manages only node-internal carriers by default; carriers on other nodes require NR Carrier Aggregation Scheduling Optimization for External SCells.
* **Core Network**: No core network dependencies.

## Limitations

* **Rebalancing Cap**: Rebalancing migrations are capped at `maxMigrationsPerRop` per carrier to protect RRC signaling capacity.
* **Score Re-evaluation Frequency**: Score re-evaluation for a connected UE occurs at most every 10 s; faster load transients are handled by the scheduler, not by carrier reassignment.
* **EMF Power Lock**: Carriers in EMF power lock (see EMF Power Lock Mid-Band) are scored but capped at their locked capacity.
* **Candidate Carrier Limit**: Maximum 8 candidate carriers evaluated per UE.
