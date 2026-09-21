---
type: concept
resource: data/vodafone-mvp/raw/Energy-Optimized Slot Allocation.pdf#network-impact
title: Network Impact
description: Analyzes the impact of Energy-Optimized Slot Allocation on energy savings,
  end-user latency, interference, and key performance indicators (KPIs).
tags:
- energy-saving
- slot-batching
- latency
- kpis
- interference
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:40:16+00:00'
  source_sha256: 6d0df0e79619eb39
sources:
- resource: data/vodafone-mvp/raw/Energy-Optimized Slot Allocation.pdf
  title: Energy-Optimized Slot Allocation
---

This section outlines the impact of the Energy-Optimized Slot Allocation feature on network energy efficiency, end-user experience, downlink interference, and key performance indicators (KPIs).

### Technical Impacts

#### Energy Savings
* **Combined Savings:** When combined with NR Micro Sleep Tx, the feature achieves an **8–15% radio energy saving** over a 24-hour period.
* **Standalone Contribution:** The primary standalone contribution of slot batching is the enlargement of sleep windows, which allows deeper or longer sleep states.

#### End-User Experience
* **Latency:** Non-delay-critical traffic experiences up to `maxBatchDelay` (default 4 ms) of additional one-way latency under low network load conditions.
* **Service Sensitivity:** Common web browsing and streaming applications are insensitive to this minor latency increase.
* **Exempt Services:** Voice and low-latency bearers are fully exempt and remain unaffected by the batching mechanism.

#### Interference Dynamics
* **Temporal Behavior:** Downlink interference becomes burstier in time due to the grouped transmission of slots.
* **Average Interference:** The average interference levels remain unchanged.
* **Link Adaptation:** Neighbor-cell link adaptation is designed to cope with this bursty behavior, though there are noted interactions with interference-aware scheduling mechanisms.

#### Key Performance Indicators (KPIs)
* **Scheduling Latency:** Average scheduling latency KPIs show a slight increase under low load conditions by design.
* **Throughput and BLER:** Both throughput and Block Error Rate (BLER) KPIs remain consistent with the network baseline.

# Cross-References

* [Feature Overview](feature-overview.md) — For more context on the general operation and architecture of Slot Batching.
* [Feature Operation](feature-operation.md) — For details on how traffic is scheduled and batched.
* [Parameters](parameters.md) — For configuring the parameters that control the feature, such as `maxBatchDelay`.
