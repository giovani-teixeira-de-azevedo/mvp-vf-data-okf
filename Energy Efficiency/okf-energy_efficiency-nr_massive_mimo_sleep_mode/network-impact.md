---
type: concept
resource: data/vodafone-mvp/raw/NR Massive MIMO Sleep Mode.pdf#network-impact
title: NETWORK IMPACT
description: Details the network impacts of sleep mode across energy consumption,
  end-user experience, mobility, and KPIs.
tags:
- energy
- end-users
- mobility
- kpis
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-25T16:14:46+00:00'
  source_sha256: 610f79760c80e9e1
sources:
- resource: data/vodafone-mvp/raw/NR Massive MIMO Sleep Mode.pdf
  title: NR Massive MIMO Sleep Mode
---

* **Energy**: primary benefit; radio unit consumption drops 20–35% during sleep.
* **End users**: connected users experience reduced peak throughput during sleep (fewer MIMO layers); latency and accessibility are unaffected. Voice (VoNR) quality is unaffected since VoNR bearers require negligible PRB resources.
* **Mobility**: neighbor cells see no change; handover and reselection behavior is unchanged because SSB coverage is preserved.
* **KPIs**: expect lower average downlink throughput per cell during sleep hours; this is by design and should be excluded from capacity dimensioning baselines.
