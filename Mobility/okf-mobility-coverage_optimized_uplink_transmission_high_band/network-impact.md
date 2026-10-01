---
type: concept
resource: data/vodafone-mvp/raw/Coverage-Optimized Uplink Transmission High-Band.pdf#network-impact
title: Network Impact
description: Details the network impact of the Coverage-Optimized Uplink Transmission
  High-Band feature across coverage, retainability, capacity, latency, and KPIs.
tags:
- coverage
- retainability
- capacity
- latency
- kpis
- uplink
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T11:14:31+00:00'
  source_sha256: 6462cb1857acb9b1
sources:
- resource: data/vodafone-mvp/raw/Coverage-Optimized Uplink Transmission High-Band.pdf
  title: Coverage-Optimized Uplink Transmission High-Band
---

This section outlines the network impact of the Coverage-Optimized Uplink Transmission High-Band feature, detailing its effects on coverage, retainability, capacity, latency, and key performance indicators (KPIs).

### Network Impact Summary

The table below summarizes the expected performance changes and resource costs associated with enabling the Coverage-Optimized Uplink Transmission High-Band feature:

| Impact Area | Description |
| :--- | :--- |
| **Coverage** | Uplink mobility signaling range is extended by 3–6 dB. An increase of 20–40% in the larger effective high-band footprint is expected for handover completion. |
| **Retainability** | Handover-related drop rate at high-band cell borders typically falls by 30–50% in scenarios where the uplink was the limiting link. |
| **Capacity** | Uplink capacity cost is proportional to the share of UEs in coverage mode. In typical macro traffic mixes, this cost is below 5% of uplink (UL) cell throughput. |
| **Latency** | Edge UEs experience up to 4 ms of additional UL latency at maximum repetition, while cell-center UEs remain unaffected. |
| **KPIs** | Expect improved handover success rates and reduced Radio Link Failure (RLF) counts on high-band cells, accompanied by a small decrease in average UL spectral efficiency (by design). |

# Cross-References

* [Feature Overview](feature-overview.md) — For an overview of the Coverage-Optimized Uplink Transmission High-Band feature.
* [Feature Operation](feature-operation.md) — For details on how the feature operates.
* [Parameters](parameters.md) — For configuration parameters related to this feature.
* [Performance Management](performance-management.md) — For monitoring the performance and KPIs of this feature.
