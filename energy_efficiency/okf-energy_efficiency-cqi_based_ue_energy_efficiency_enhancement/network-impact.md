---
type: concept
resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf#network-impact
title: Network Impact
description: Details the network-wide impacts, capacity considerations, and KPI changes
  resulting from CQI-Based UE Energy Efficiency Enhancement.
tags:
- network-impact
- cqi
- energy-efficiency
- kpis
- throughput
- signaling
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:38:41+00:00'
  source_sha256: 79b9ac9209d185ca
sources:
- resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf
  title: CQI-Based UE Energy Efﬁciency Enhancement
---

This section outlines the impact of the CQI-Based UE Energy Efficiency Enhancement feature on end-user performance, cell capacity, signaling overhead, and key performance indicators (KPIs).

### End Users
* **Connected-Mode Modem Energy:** Typically yields **5–12% lower** connected-mode modem energy consumption for typical smartphone traffic.
* **High-CQI UEs:** Experience no throughput impact as they receive the same amount of data in less time.
* **Low-CQI UEs:** Experience marginally lower peak throughput (by one to two MCS steps) in exchange for fewer retransmissions.

### Capacity
* **PRB Utilization:** Instantaneous Physical Resource Block (PRB) utilization becomes burstier, while average utilization remains approximately unchanged.
* **Congested Cells:** Cells running near congestion receive negligible benefit from this feature because compaction opportunities vanish under load.

### Signaling
* **RRC Reconfiguration:** A small increase in Radio Resource Control (RRC) reconfiguration volume occurs when UEs cross CQI class boundaries. This signaling overhead is bounded by the hysteresis design of the feature.

### KPIs
* **UE Active Time:** Expect a reduction in the average UE active time per data burst.
* **HARQ Retransmissions:** Expect a reduction in the downlink HARQ (Hybrid Automatic Repeat Request) retransmission rate for the low-CQI user population.

# Cross-References
* [Feature Overview](feature-overview.md) — For an overview of how the energy efficiency mechanism is designed.
* [Feature Operation](feature-operation.md) — Details on how CQI boundaries and hysteresis are managed.
* [Performance Management](performance-management.md) — Metrics and KPIs for observing and assessing feature impact.
