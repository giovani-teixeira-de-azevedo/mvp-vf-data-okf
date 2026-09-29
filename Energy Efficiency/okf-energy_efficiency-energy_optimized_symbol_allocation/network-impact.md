---
type: concept
resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf#network-impact
title: NETWORK IMPACT
description: Energy, end-user, capacity, and KPI impact of Energy-Optimized Symbol
  Allocation.
tags:
- energy-saving
- network-impact
- kpi
- capacity
- latency
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-29T22:48:26+00:00'
  source_sha256: 131b946f900cf11e
sources:
- resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf
  title: Energy-Optimized Symbol Allocation
---

- **Energy**: 3–7% incremental radio energy saving on top of slot-level features, concentrated at low-to-medium load.
- **End users**: None observable. Per-packet air-interface latency actually improves marginally (transmissions complete earlier in the slot).
- **Capacity**: At medium load, frequency-domain blocking increases slightly; the `compactionLoadThr` guard confines operation to load levels where this is immaterial.
- **KPIs**: Average occupied symbols per active slot drops visibly; throughput, BLER, and latency KPIs remain at baseline.

# Cross-References

- [PARAMETERS](parameters.md)
