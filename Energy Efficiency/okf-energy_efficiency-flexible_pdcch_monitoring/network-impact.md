---
type: concept
resource: data/vodafone-mvp/raw/Flexible PDCCH Monitoring.pdf#network-impact
title: Network Impact
description: Details the network, end-user, capacity, signaling, and KPI impacts of
  Flexible PDCCH Monitoring.
tags:
- pdcch
- network-impact
- kpi
- signaling
- sssg
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-30T14:19:55+00:00'
  source_sha256: 1650f57b185cf8c9
sources:
- resource: data/vodafone-mvp/raw/Flexible PDCCH Monitoring.pdf
  title: Flexible PDCCH Monitoring
---

This section outlines the network impact of the Flexible PDCCH Monitoring feature across end-user experience, capacity, coverage, signaling, and Key Performance Indicators (KPIs).

## Impact Breakdown

- **End users**: 10–20% additional connected-mode modem energy reduction for bursty traffic; first-packet latency after quiet periods increases by up to the sparse period (≤ 2 ms at default settings) — imperceptible for the eligible traffic classes.
- **Capacity and coverage**: None. PDCCH capacity is marginally relieved because sparse UEs occupy fewer blind-decode candidates per slot, which slightly benefits cells near PDCCH congestion (see PDCCH Capacity Boost).
- **Signaling**: Search Space Set Group (SSSG) configuration adds a small one-time RRC payload at connection setup; switching itself is layer-1 and costs no RRC signaling.
- **KPIs**: No change expected in accessibility, retainability, or throughput KPIs; latency percentile KPIs for best-effort traffic shift by at most the sparse period.
