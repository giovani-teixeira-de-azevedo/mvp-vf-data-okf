---
type: concept
resource: data/vodafone-mvp/raw/Fronthaul Sharing.pdf#network-impact
title: Network Impact
description: Details the operational and physical impact of Fronthaul Sharing on deployment,
  availability, latency, and capacity.
tags:
- fronthaul-sharing
- network-impact
- deployment
- availability
- latency
- capacity
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:43:37+00:00'
  source_sha256: a2b9d2925c57a4a8
sources:
- resource: data/vodafone-mvp/raw/Fronthaul Sharing.pdf
  title: Fronthaul Sharing
---

This section outlines the operational and physical impacts of Fronthaul Sharing on the radio access network, specifically addressing deployment feasibility, service availability, latency constraints, and capacity dimensioning.

## Key Impacts

*   **Deployment**: NR overlays can be deployed without requiring new fiber installations on most sites. This reduces the typical site upgrade effort by **40% to 70%**.
*   **Availability**: Fronthaul sharing introduces a shared-fate dependency. A host baseband failure or restart will take down guest carriers on the shared paths. This dependency must be factored into availability calculations and maintenance window planning.
*   **Latency**: The added fronthaul delay is bounded (in the microsecond range) and is automatically compensated. There is no user-perceivable impact on latency.
*   **Capacity**: No capacity impact occurs under correct dimensioning. However, an oversubscribed shared eCPRI link degrades both hosts. To prevent this, policing functions and dimensioning counters are required.

# Cross-References

*   [Feature Overview](feature-overview.md) — For general overview and context on Fronthaul Sharing.
*   [Performance Management](performance-management.md) — For the dimensioning counters used to monitor link capacity.
*   [Parameters](parameters.md) — For configuring the policing and sharing functions.
