---
type: concept
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#network-impact
title: NETWORK IMPACT
description: Network impact of Closed-Loop Power Control High-Band on uplink interference,
  throughput, UE battery, signaling, and KPIs.
tags:
- network-impact
- uplink-interference
- throughput
- ue-battery
- signaling
- kpis
- fr2
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T15:15:15+00:00'
  source_sha256: 615eb1ed5a388d7f
sources:
- resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
  title: Closed-Loop Power Control High-Band
---

- **Uplink interference**: average IoT on loaded FR2 cells drops by 1.5–3 dB; neighbor high-band cells benefit as well.
- **Throughput**: uplink cell-edge throughput improves 10–20%; cell-center throughput is essentially unchanged since those UEs were already SINR-limited by MCS caps.
- **UE battery**: average UE uplink transmit power is reduced, with a measurable battery saving for FWA terminals with sustained uplink traffic.
- **Signaling**: TPC commands ride in existing DCI; there is no additional control channel load.
- **KPIs**: expect a small transient increase in uplink BLER during the first days as loops converge network-wide; this settles once `pcTargetSinr` is tuned.

# Cross-References

- [PARAMETERS](parameters.md)
