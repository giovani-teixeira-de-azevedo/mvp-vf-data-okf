---
type: concept
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#network-impact
title: NETWORK IMPACT
description: Overview of network impacts covering uplink interference, throughput,
  UE battery, signaling, and KPIs.
tags:
- network-impact
- uplink-interference
- throughput
- ue-battery
- signaling
- kpis
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T13:00:46+00:00'
  source_sha256: 615eb1ed5a388d7f
sources:
- title: Closed-Loop Power Control High-Band
  resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
---

This section details network impacts across uplink interference, throughput, UE battery, signaling, and KPIs.

## Network Impact

- **Uplink interference:** Average IoT on loaded FR2 cells drops by 1.5–3 dB; neighbor high-band cells benefit as well.
- **Throughput:** Uplink cell-edge throughput improves 10–20%; cell-center throughput is essentially unchanged since those UEs were already SINR-limited by MCS caps.
- **UE battery:** Average UE uplink transmit power is reduced, with a measurable battery saving for FWA terminals with sustained uplink traffic.
- **Signaling:** TPC commands ride in existing DCI; there is no additional control channel load.
- **KPIs:** Expect a small transient increase in uplink BLER during the first days as loops converge network-wide; this settles once `pcTargetSinr` is tuned.

# Cross-References

- [Parameters](parameters.md)
