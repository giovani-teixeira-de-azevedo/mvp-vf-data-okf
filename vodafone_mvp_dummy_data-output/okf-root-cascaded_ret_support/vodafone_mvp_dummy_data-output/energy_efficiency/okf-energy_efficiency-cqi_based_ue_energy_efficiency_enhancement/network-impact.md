---
type: concept
resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf#network-impact
title: NETWORK IMPACT
description: Details the impact of the CQI-Based UE Energy Efficiency Enhancement
  feature on end users, capacity, signaling, and KPIs.
tags:
- network-impact
- cqi
- energy-efficiency
- kpis
- prb-utilization
- rrc-signaling
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T14:17:37+00:00'
  source_sha256: 79b9ac9209d185ca
sources:
- resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf
  title: CQI-Based UE Energy Efficiency Enhancement
---

This section details the network impact of the CQI-Based UE Energy Efficiency Enhancement feature on end users, cell capacity, network signaling, and Key Performance Indicators (KPIs).

## Network Impact Areas

- **End users**: 5–12% lower connected-mode modem energy for typical smartphone traffic; no throughput impact for high-CQI UEs (they receive the same data in less time). Low-CQI UEs see marginally lower peak throughput (one to two MCS steps) in exchange for fewer retransmissions.
- **Capacity**: Instantaneous PRB utilization becomes burstier; average utilization is approximately unchanged. Cells running near congestion see negligible benefit because compaction opportunities vanish under load.
- **Signaling**: A small increase in RRC reconfiguration volume when UEs cross CQI class boundaries; bounded by the hysteresis design.
- **KPIs**: Expect a reduction in average UE active time per data burst and in downlink HARQ retransmission rate for the low-CQI population.
