---
type: concept
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#network-impact
title: Network Impact
description: Overview of the network impacts associated with Closed-Loop Power Control
  High-Band, including interference, throughput, UE battery, signaling, and KPIs.
tags:
- power-control
- high-band
- fr2
- network-impact
- kpis
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T12:53:57+00:00'
  source_sha256: 615eb1ed5a388d7f
sources:
- resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
  title: Closed-Loop Power Control High-Band
---

This section details the impact on network performance, UE battery consumption, signaling load, and KPIs resulting from the implementation of Closed-Loop Power Control High-Band.

## Network Impacts

- **Uplink interference:** Average Interference over Thermal (IoT) on loaded FR2 cells drops by 1.5–3 dB; neighboring high-band cells benefit as well.
- **Throughput:** Uplink cell-edge throughput improves by 10–20%. Cell-center throughput is essentially unchanged since those UEs were already SINR-limited by MCS caps.
- **UE battery:** Average UE uplink transmit power is reduced, providing measurable battery savings for Fixed Wireless Access (FWA) terminals with sustained uplink traffic.
- **Signaling:** Transmit Power Control (TPC) commands ride in existing Downlink Control Information (DCI); there is no additional control channel load.
- **KPIs:** A small transient increase in uplink Block Error Rate (BLER) is expected during the first days as loops converge network-wide. This settles once `pcTargetSinr` is tuned.

# Cross-References

- [Parameters](parameters.md)
