---
type: concept
resource: data/vodafone-mvp/raw/NR Massive MIMO Sleep Mode.pdf#network-impact
title: Network Impact
description: Overview of the impact of NR Massive MIMO Sleep Mode on energy savings,
  end-user experience, mobility, and key performance indicators.
tags:
- Massive MIMO
- Sleep Mode
- Energy Saving
- KPI
- Network Impact
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:42:02+00:00'
  source_sha256: 610f79760c80e9e1
sources:
- title: NR Massive MIMO Sleep Mode
  resource: data/vodafone-mvp/raw/NR Massive MIMO Sleep Mode.pdf
---

This section outlines the network impact of the NR Massive MIMO Sleep Mode feature, detailing its effects on energy consumption, user experience, mobility, and Key Performance Indicators (KPIs).

### Impact Details

*   **Energy Consumption:** 
    *   This is the primary benefit of the feature.
    *   Radio unit (RU) power consumption drops by **20% to 35%** during sleep mode.
*   **End-User Experience:** 
    *   Connected users experience **reduced peak throughput** during sleep periods due to the utilization of fewer MIMO layers.
    *   Latency and accessibility are unaffected.
    *   Voice over NR (VoNR) quality is unaffected since VoNR bearers require negligible Physical Resource Block (PRB) resources.
*   **Mobility:** 
    *   Neighboring cells experience no changes.
    *   Handover and cell reselection behaviors remain unchanged because SSB (Synchronization Signal Block) coverage is preserved.
*   **Key Performance Indicators (KPIs):** 
    *   An expected reduction in the average downlink throughput per cell will occur during sleep hours.
    *   This reduction is by design and should be excluded from capacity dimensioning baselines.

# Cross-References

*   [Feature Overview](feature-overview.md) - For context on the sleep mode feature and its general operation.
*   [Feature Operation](feature-operation.md) - For details on the trigger criteria and activation of sleep states.
*   [Performance Management](performance-management.md) - For observability and KPI monitoring.
