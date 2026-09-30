---
type: concept
resource: data/vodafone-mvp/raw/Energy-Optimized Slot Allocation.pdf#network-impact
title: NETWORK IMPACT
description: Summarizes network impacts including energy savings, end-user latency,
  interference behavior, and KPIs.
tags:
- network-impact
- energy
- latency
- interference
- kpi
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-30T09:42:18+00:00'
  source_sha256: 6d0df0e79619eb39
sources:
- resource: data/vodafone-mvp/raw/Energy-Optimized Slot Allocation.pdf
  title: Energy-Optimized Slot Allocation
---

This section outlines the network impact across energy savings, end users, interference, and KPIs.

* **Energy:** 8–15% radio energy saving over 24 hours when combined with NR Micro Sleep Tx; the standalone contribution of slot batching is the enlargement of sleep windows.
* **End users:** non-delay-critical traffic gains up to `maxBatchDelay` (default 4 ms) of additional one-way latency at low load; web browsing and streaming are insensitive to this. Voice and low-latency bearers are exempt and unaffected.
* **Interference:** downlink interference becomes burstier in time; average interference is unchanged. Neighbor-cell link adaptation copes with this by design, but see the noted interaction with interference-aware scheduling.
* **KPIs:** average scheduling latency KPIs increase slightly at low load by design; throughput and BLER KPIs remain at baseline.

# Cross-References

* [Parameters](parameters.md)
* [Feature Operation](feature-operation.md)
