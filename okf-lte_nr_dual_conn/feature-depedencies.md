---
type: concept
resource: data/vodafone-mvp/raw/LTE-NR_Dual_Conn.pdf#feature-depedencies
title: Feature Dependencies
description: Outlines the licensing, node configuration, transport, and feature dependencies
  required for EN-DC operation.
tags:
- LTE-NR Dual Connectivity
- EN-DC
- Feature Dependencies
- Licensing
- X2 Configuration
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-30T12:27:13+00:00'
  source_sha256: 942b31b3e9ff33e1
sources:
- resource: data/vodafone-mvp/raw/LTE-NR_Dual_Conn.pdf
  title: LTE-NR Dual Connectivity
---

This section outlines the prerequisites, licensing, transport, and related feature dependencies required for E-UTRA-NR Dual Connectivity (EN-DC) activation.

## EN-DC Integration Prerequisites

EN-DC couples four core elements, all of which must be verified prior to feature activation:

- LTE node
- NR node
- X2 transport between the LTE and NR nodes
- EPC upgrade level

The most common integration faults encountered are:

- X2-U MTU mismatches
- Missing EPC support for NR extensions

## Feature Dependencies

- **Licensing and Control:** Requires a valid license key (`FAK-33215`) and `FeatureCtrl=LteNrDualConnectivity` set to `ACTIVATED` on both the LTE and NR nodes.
- **X2 Configuration:** Requires X2 Configuration (EN-DC) for X2-C/X2-U setup between the anchor eNodeB and the gNodeB.
- **Downlink and Uplink Aggregation:** LTE-NR Downlink Aggregation and LTE-NR Uplink Aggregation extend this feature with dual-leg user-plane transmission; without them, downlink data is carried on the NR leg with X2 fallback.
- **IMS Voice:** EPS Fallback for IMS Voice is recommended so that voice is served on LTE while EN-DC handles data.

# Cross-References

- [Feature Overview](feature-overview.md)
