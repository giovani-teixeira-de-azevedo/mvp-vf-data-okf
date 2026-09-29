---
type: concept
resource: data/vodafone-mvp/raw/LTE-NR_Dual_Conn.pdf#feature-depedencies
title: Feature Dependencies
description: Outlines technical prerequisites, licensing, configuration, and feature
  dependencies for LTE-NR Dual Connectivity (EN-DC).
tags:
- EN-DC
- LTE-NR
- Dependencies
- Licensing
- X2
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-29T23:17:40+00:00'
  source_sha256: 942b31b3e9ff33e1
sources:
- title: LTE-NR Dual Connectivity
  resource: data/vodafone-mvp/raw/LTE-NR_Dual_Conn.pdf
---

This section details the prerequisites, configuration requirements, and feature interactions necessary for activating LTE-NR Dual Connectivity (EN-DC).

EN-DC couples an LTE node, an NR node, the X2 transport between them, and the EPC upgrade level. All four legs must be verified before activation; the most common integration faults are X2-U MTU mismatches and missing EPC support for the NR extensions.

## Feature Dependencies

- **Licensing and Activation**: Requires a valid license key (`FAK-33215`) and `FeatureCtrl=LteNrDualConnectivity` set to `ACTIVATED` on both the LTE and NR nodes.
- **X2 Configuration**: Requires X2 Configuration (EN-DC) for X2-C/X2-U setup between the anchor eNodeB and the gNodeB.
- **Data Aggregation**: LTE-NR Downlink Aggregation and LTE-NR Uplink Aggregation extend this feature with dual-leg user-plane transmission; without them, downlink data is carried on the NR leg with X2 fallback.
- **Voice Support**: EPS Fallback for IMS Voice is recommended so that voice is served on LTE while EN-DC handles data.

# Cross-References

- [Feature Overview](feature-overview.md)
