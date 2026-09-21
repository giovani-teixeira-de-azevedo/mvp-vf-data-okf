---
type: concept
resource: data/vodafone-mvp/raw/Flexible PDCCH Monitoring.pdf#network-impact
title: Network Impact
description: Analyzes the network impact of Flexible PDCCH Monitoring on end-user
  modem energy, capacity, signaling, and KPIs.
tags:
- PDCCH
- Network Impact
- Energy Saving
- KPI
- Signaling
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:41:35+00:00'
  source_sha256: 1650f57b185cf8c9
sources:
- resource: data/vodafone-mvp/raw/Flexible PDCCH Monitoring.pdf
  title: Flexible PDCCH Monitoring
---

The **Flexible PDCCH Monitoring** feature introduces changes to modem energy consumption, signaling overhead, cell capacity, and overall network performance. This section details the expected impacts across these areas.

### Network Impact Overview

| Impact Area | Details and Expected Effects |
| :--- | :--- |
| **End Users** | <ul><li>**Modem Energy:** Provides a 10–20% additional connected-mode modem energy reduction for bursty traffic.</li><li>**Latency:** First-packet latency after quiet periods increases by up to the sparse period ($\le 2\text{ ms}$ at default settings). This impact is imperceptible for eligible traffic classes.</li></ul> |
| **Capacity & Coverage** | <ul><li>No capacity or coverage degradation.</li><li>PDCCH capacity is marginally relieved because sparse UEs occupy fewer blind-decode candidates per slot, which slightly benefits cells experiencing or near PDCCH congestion.</li></ul> |
| **Signaling** | <ul><li>**RRC Payload:** SSSG configuration adds a small, one-time RRC payload at connection setup.</li><li>**Switching Overhead:** SSSG switching itself is performed at Layer-1 (L1) and costs no RRC signaling.</li></ul> |
| **KPIs** | <ul><li>**Accessibility & Retainability:** No change expected in these KPIs.</li><li>**Throughput:** No change expected in throughput KPIs.</li><li>**Latency KPIs:** Latency percentile KPIs for best-effort traffic shift by at most the duration of the sparse period.</li></ul> |

# Cross-References

* [Feature Overview](feature-overview.md)
* [Feature Operation](feature-operation.md)
