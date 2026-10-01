---
type: concept
resource: data/vodafone-mvp/raw/EPS Fallback for IMS Voice.pdf#network-impact
title: Network Impact
description: Analyzes the impact of EPS Fallback on voice services, call setup delay,
  NR KPIs, LTE layer, and signaling.
tags:
- EPS Fallback
- IMS Voice
- Network Impact
- VoLTE
- VoNR
- NR KPIs
- LTE
- Signaling
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T10:15:10+00:00'
  source_sha256: 7cab3c778ecea635
sources:
- title: EPS Fallback for IMS Voice
  resource: data/vodafone-mvp/raw/EPS Fallback for IMS Voice.pdf
---

This section outlines the network impact of implementing EPS Fallback for IMS Voice, detailing its effects on voice services, call setup delay, NR KPIs, the LTE layer, and signaling overhead.

## Network Impact Analysis

The deployment of EPS Fallback introduces several key impacts across the 5G Standalone (SA) and LTE network layers:

*   **Voice Service:** Enables IMS voice for all 5G SA subscribers regardless of VoNR readiness. Once target data is clean, call setup success is on par with VoLTE.
*   **Call Setup Delay:** Adds additional delay to voice call establishment compared to native VoNR:
    *   **Handover method:** Adds 0.3–0.8 seconds.
    *   **Redirection method:** Adds 1–2 seconds.
*   **NR KPIs:** Each fallback event is registered as an outgoing inter-RAT mobility event rather than a call drop. NR traffic time and throughput on voice-heavy cells decrease because voice users move to LTE for the duration of the call.
*   **LTE Layer:** VoLTE traffic and QCI 1 bearer load increase correspondingly. It is necessary to verify LTE capacity headroom in high-traffic areas.
*   **Signaling:** AMF–MME (N26) signaling increases when using the handover method, which requires appropriate dimensioning.

# Cross-References

*   [Feature Overview](feature-overview.md)
*   [Feature Operation](feature-operation.md)
