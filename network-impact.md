---
type: concept
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#network-impact
title: NETWORK IMPACT
description: Summary of network impact on interference, throughput, UE battery, signaling,
  and KPIs for Closed-Loop Power Control High-Band.
tags:
- network-impact
- closed-loop-power-control
- fr2
- uplink-interference
- throughput
- ue-battery
- signaling
- kpis
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-24T14:32:35+00:00'
  source_sha256: 615eb1ed5a388d7f
sources:
- title: Closed-Loop Power Control High-Band
  resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
---

This section details the network impact resulting from enabling Closed-Loop Power Control High-Band, covering uplink interference, throughput, UE battery savings, signaling overhead, and key performance indicators.

### Network Impact Details

- **Uplink interference**: Average Interference over Thermal (IoT) on loaded FR2 cells drops by 1.5–3 dB; neighbor high-band cells benefit as well.
- **Throughput**: Uplink cell-edge throughput improves by 10–20%; cell-center throughput is essentially unchanged since those UEs were already SINR-limited by MCS caps.
- **UE battery**: Average UE uplink transmit power is reduced, with a measurable battery saving for FWA terminals with sustained uplink traffic.
- **Signaling**: TPC commands ride in existing DCI; there is no additional control channel load.
- **KPIs**: Expect a small transient increase in uplink BLER during the first days as loops converge network-wide; this settles once `pcTargetSinr` is tuned.

# Cross-References

- [PARAMETERS](parameters.md) - Parameters associated with this feature, including `pcTargetSinr`.
