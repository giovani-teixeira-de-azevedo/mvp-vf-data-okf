---
type: concept
resource: data/vodafone-mvp/raw/NR Automated Neighbor Relations.pdf#network-impact
title: Network Impact
description: Overview of the impact of NR Automated Neighbor Relations (ANR) on mobility
  quality, operational cost, UE performance, signaling, and KPIs.
tags:
- ANR
- Network Impact
- Mobility
- KPI
- UE Impact
- Signaling
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T16:53:43+00:00'
  source_sha256: 92437beaf09e28a5
sources:
- resource: data/vodafone-mvp/raw/NR Automated Neighbor Relations.pdf
  title: NR Automated Neighbor Relations
---

This section describes the network impact of activating the NR Automated Neighbor Relations (ANR) feature, detailing its effects on mobility quality, operational costs, User Equipment (UE) performance, signaling overhead, and Key Performance Indicators (KPIs).

The activation of NR Automated Neighbor Relations (ANR) impacts several key areas of network performance and operations:

*   **Mobility Quality:** Handover failures due to missing neighbors are effectively eliminated. A measurable improvement in overall handover success can be expected within days of activation in a growing network.
*   **Operational Cost:** Manual neighbor planning and audit effort is removed, typically saving several engineer-hours per site over its lifetime.
*   **UE Impact:** Cell Global Identifier (CGI) reads briefly interrupt the reporting UE's scheduling (using autonomous gaps of up to 160 ms). However, the per-cell order cap keeps the aggregate throughput impact below 0.5%.
*   **Signaling:** A modest volume of NG Configuration Transfer signaling occurs during network growth phases, and a burst of Xn setups follows new-site integration.
*   **KPIs:** Neighbor-count and relation-churn statistics become available. Handover KPIs improve or hold steady and never degrade, since ANR only adds usable relations.

# Cross-References

*   [Feature Overview](feature-overview.md) — For an overview of the ANR feature and its capabilities.
*   [Parameters](parameters.md) — For configuration parameters related to ANR operation.
*   [Performance Management](performance-management.md) — For details on KPIs and performance counters.
