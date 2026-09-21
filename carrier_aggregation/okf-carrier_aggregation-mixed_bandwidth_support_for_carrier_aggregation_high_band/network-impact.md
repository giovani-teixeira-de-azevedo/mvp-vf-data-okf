---
type: concept
resource: data/vodafone-mvp/raw/Mixed Bandwidth Support for Carrier Aggregation High-Band.pdf#network-impact
title: Network Impact
description: Detailed analysis of the network impact of the Mixed Bandwidth Support
  for Carrier Aggregation High-Band feature on throughput, capacity, signaling, scheduling,
  and KPIs.
tags:
- carrier-aggregation
- network-impact
- kpi
- throughput
- scheduling
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:38:29+00:00'
  source_sha256: 7572cb5a1fc97b19
sources:
- title: Mixed Bandwidth Support for Carrier Aggregation High-
  resource: data/vodafone-mvp/raw/Mixed Bandwidth Support for Carrier Aggregation
    High-Band.pdf
---

The "Mixed Bandwidth Support for Carrier Aggregation High-Band" feature introduces key impacts across multiple network metrics and areas, including throughput, capacity, signaling, baseband scheduling complexity, and KPI evaluation.

## Detailed Network Impact

* **Peak Throughput**: Capable User Equipments (UEs) gain access to the full bandwidth of previously excluded Component Carriers (CCs). The typical peak throughput gain ranges from **15% to 40%**, depending on the holding shape.
* **Capacity**: During busy-hour periods, downlink capacity rises proportionally to the recovered spectrum.
* **Signaling**: There is a negligible change in signaling because the combination builder runs exclusively at configuration time.
* **Scheduling Complexity**: The baseband load increases slightly (approximately **2%**) due to per-CC normalization on wide combinations.
* **KPIs**: Per-CC throughput Key Performance Indicators (KPIs) become structurally unequal across carriers of different sizes. It is recommended to normalize per MHz when comparing carriers.

# Cross-References
* [Feature Overview](feature-overview.md) - Describes the overall capability of the feature.
* [Performance Management](performance-management.md) - Contains details regarding KPI monitoring and management.
