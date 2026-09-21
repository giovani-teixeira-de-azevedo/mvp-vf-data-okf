---
type: concept
resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf#network-impact
title: Network Impact
description: Outlines the impacts of the Energy-Optimized Symbol Allocation feature
  on energy consumption, end users, capacity, and key performance indicators (KPIs).
tags:
- energy-saving
- network-impact
- kpis
- latency
- capacity
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:41:00+00:00'
  source_sha256: 131b946f900cf11e
sources:
- title: Energy-Optimized Symbol Allocation
  resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf
---

This section details the impact of the Energy-Optimized Symbol Allocation feature on energy consumption, end-user experience, network capacity, and Key Performance Indicators (KPIs).

### Key Performance and Network Impacts

*   **Energy**: 
    *   Provides a **3% to 7% incremental radio energy saving** on top of slot-level features.
    *   Savings are concentrated at low-to-medium load levels.
*   **End Users**: 
    *   No observable negative impact.
    *   Per-packet air-interface latency improves marginally as transmissions complete earlier in the slot.
*   **Capacity**: 
    *   At medium load, frequency-domain blocking increases slightly.
    *   The `compactionLoadThr` guard parameter confines feature operation to load levels where this blocking is immaterial.
*   **KPIs**: 
    *   The average occupied symbols per active slot drops visibly.
    *   Throughput, BLER, and latency KPIs remain at baseline.

# Cross-References

*   [Parameters](parameters.md) — Contains the `compactionLoadThr` guard parameter used to regulate feature operation.
