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
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-30T17:15:55+00:00'
  source_sha256: 131b946f900cf11e
sources:
- title: Energy-Optimized Symbol Allocation
  resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf
---

This section describes the network impact of the Energy-Optimized Symbol Allocation feature, detailing its effects on energy consumption, end-user experience, network capacity, and key performance indicators (KPIs).

The observed impacts of the feature include:

*   **Energy:** Provides a 3–7% incremental radio energy saving on top of slot-level features, which is concentrated at low-to-medium load levels.
*   **End Users:** No observable negative impact. Per-packet air-interface latency actually improves marginally because transmissions complete earlier in the slot.
*   **Capacity:** At medium load, frequency-domain blocking increases slightly. The [compactionLoadThr](parameters.md) guard parameter confines operation to load levels where this blocking is immaterial.
*   **KPIs:** The average occupied symbols per active slot drops visibly, while throughput, BLER, and latency KPIs remain at baseline levels.

# Cross-References

*   [PARAMETERS](parameters.md) - Details the parameters governing the feature, including `compactionLoadThr`.
*   [FEATURE OPERATION](feature-operation.md) - Explains how the symbol allocation and compaction mechanisms operate.
