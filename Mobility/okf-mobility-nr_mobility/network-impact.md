---
type: concept
resource: data/vodafone-mvp/raw/NR Mobility.pdf#network-impact
title: Network Impact
description: Analyzes the impact of NR mobility on service continuity, user plane
  performance, signaling load, measurement overhead, and network KPIs.
tags:
- mobility
- nr
- handover
- signaling
- kpi
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T09:18:40+00:00'
  source_sha256: f8a5d71b78bbcf95
sources:
- title: NR Mobility
  resource: data/vodafone-mvp/raw/NR Mobility.pdf
---

This section outlines the network-wide impacts of the NR Mobility feature, detailing its effects on service continuity, user plane performance, signaling overhead, measurement costs, and key performance indicators (KPIs).

The implementation of NR Mobility has direct implications across several key areas of network performance and resource utilization:

*   **Service Continuity:** This feature forms the fundamental basis of session continuity in NR. Without active mobility mechanisms, UEs would drop connections at every cell boundary and would have to recover via RRC re-establishment or idle-mode cell reselection.
*   **User Plane Impact:**
    *   **Xn Handover:** Introduces a 30–60 ms user plane interruption.
    *   **NG Handover:** Introduces a 60–120 ms user plane interruption.
    *   **Data Loss:** Handover is lossless for Acknowledged Mode (AM) Data Radio Bearers (DRBs) through the use of data forwarding. However, there is a potential for single-packet loss on Unacknowledged Mode (UM) DRBs.
*   **Signaling Load:** Each handover execution generates one Xn or NG preparation exchange in addition to a path switch. AMF signaling capacity must be dimensioned to handle the network's aggregate handover rate (with typical busy-hour handovers per UE ranging from 5 to 15 in urban macro environments).
*   **Measurement Cost:** Configuring inter-frequency measurement gaps reduces a gapped UE's throughput by up to approximately 7% (assuming a 40 ms gap period). However, utilizing A2-gated activation (where measurements are only triggered when serving cell quality drops below a threshold) ensures that the affected UE population remains small.
*   **KPIs:** Handover success rate, ping-pong rate, and user plane interruption time serve as the core mobility health indicators for the network.

# Cross-References

*   [Feature Overview](feature-overview.md) — General overview of the NR Mobility feature.
*   [Feature Operation](feature-operation.md) — Detailed operational mechanisms of NR Mobility.
*   [Performance Management](performance-management.md) — KPIs and counters used to monitor mobility performance.
*   [Parameters](parameters.md) — Configuration parameters that influence mobility behavior and thresholds.
