---
type: concept
resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf#network-impact
title: Network Impact
description: Details the impact of the CQI-Based UE Energy Efficiency Enhancement
  feature on end users, cell capacity, signaling, and network KPIs.
tags:
- network-impact
- cqi
- energy-efficiency
- prb-utilization
- rrc-reconfiguration
- harq
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-25T17:14:51+00:00'
  source_sha256: 79b9ac9209d185ca
sources:
- resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf
  title: CQI-Based UE Energy Efficiency Enhancement
---

This section describes the network impact of the CQI-Based UE Energy Efficiency Enhancement feature across end-user experience, network capacity, signaling volume, and key performance indicators (KPIs).

## Impact Areas

* **End users:**
  * **Modem Energy:** 5–12% lower connected-mode modem energy for typical smartphone traffic.
  * **High-CQI UEs:** No throughput impact, as they receive the same data in less time.
  * **Low-CQI UEs:** Marginally lower peak throughput (one to two MCS steps) in exchange for fewer retransmissions.
* **Capacity:**
  * **PRB Utilization:** Instantaneous PRB utilization becomes burstier; average utilization is approximately unchanged.
  * **Congested Cells:** Cells running near congestion see negligible benefit because compaction opportunities vanish under load.
* **Signaling:**
  * **RRC Reconfiguration Volume:** A small increase in RRC reconfiguration volume occurs when UEs cross CQI class boundaries, bounded by the hysteresis design.
* **KPIs:**
  * Expect a reduction in average UE active time per data burst.
  * Expect a reduction in downlink HARQ retransmission rate for the low-CQI population.
