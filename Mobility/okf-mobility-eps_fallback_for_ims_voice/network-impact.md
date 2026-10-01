---
type: concept
resource: data/vodafone-mvp/raw/EPS Fallback for IMS Voice.pdf#network-impact
title: Network Impact
description: Analyzes the impact of EPS Fallback on voice services, call setup delay,
  NR KPIs, LTE layer capacity, and signaling load.
tags:
- eps-fallback
- ims-voice
- network-impact
- vonr
- lte
- nr-kpis
- signaling
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T11:01:26+00:00'
  source_sha256: 7cab3c778ecea635
sources:
- resource: data/vodafone-mvp/raw/EPS Fallback for IMS Voice.pdf
  title: EPS Fallback for IMS Voice
---

This section outlines the network impacts of implementing EPS Fallback for IMS Voice. It details the effects on voice service availability, call setup delay, NR KPIs, LTE layer capacity, and signaling load.

## Network Impact Areas

The implementation of EPS Fallback affects several areas of the network:

*   **Voice Service**: Enables IMS voice for all Standalone (SA) subscribers regardless of Voice over New Radio (VoNR) readiness. Call setup success is on par with Voice over LTE (VoLTE) once target data is clean.
*   **Call Setup Delay**: Adds additional delay to voice call establishment compared to native VoNR:
    *   **Handover method**: Adds 0.3–0.8 seconds.
    *   **Redirect method**: Adds 1–2 seconds.
*   **NR KPIs**: Each fallback appears as an outgoing inter-RAT mobility event rather than a call drop. NR traffic time and throughput on voice-heavy cells decrease as voice users move to LTE for the duration of the call.
*   **LTE Layer**: VoLTE traffic and QCI 1 bearer load increase correspondingly. It is necessary to verify LTE capacity headroom in high-traffic areas.
*   **Signaling**: AMF–MME (N26) signaling increases when using the handover method, requiring appropriate dimensioning.

# Cross-References

*   [Feature Overview](feature-overview.md) — For an overview of EPS Fallback for IMS Voice.
*   [Feature Operation](feature-operation.md) — For details on the handover and redirect methods.
*   [Performance Management](performance-management.md) — For information on monitoring NR KPIs and mobility events.
