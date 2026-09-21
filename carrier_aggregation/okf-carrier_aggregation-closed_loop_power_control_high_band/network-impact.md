---
type: concept
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#network-impact
title: Network Impact
description: Analyzes the network-wide effects of Closed-Loop Power Control High-Band
  on uplink interference, throughput, UE battery, signaling, and KPIs.
tags:
- RAN
- Power Control
- Network Impact
- FR2
- FWA
- KPI
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:37:01+00:00'
  source_sha256: 615eb1ed5a388d7f
sources:
- resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
  title: Closed-Loop Power Control High-Band
---

This section details the network-wide impacts of the Closed-Loop Power Control High-Band feature on key parameters, including interference, throughput, signaling, battery life, and key performance indicators (KPIs).

## Key Areas of Impact

### Uplink Interference
* **FR2 Cells**: Average Interference over Thermal (IoT) on loaded FR2 cells drops by **1.5–3 dB**.
* **Neighboring Cells**: Neighboring high-band cells benefit from reduced inter-cell interference as well.

### Throughput
* **Uplink Cell-Edge Throughput**: Improves by **10–20%** due to optimized power levels and reduced interference.
* **Cell-Center Throughput**: Remains essentially unchanged. These User Equipments (UEs) are already SINR-limited by Modulation and Coding Scheme (MCS) caps and do not see throughput gains from this feature.

### UE Battery Consumption
* **Average Transmit Power**: The average UE uplink transmit power is reduced.
* **FWA Terminals**: A measurable battery saving is observed, particularly for Fixed Wireless Access (FWA) terminals with sustained uplink traffic.

### Control Signaling
* **Control Channel Load**: There is no additional control channel overhead or load introduced.
* **TPC Delivery**: Transmit Power Control (TPC) commands are carried within the existing Downlink Control Information (DCI) formats.

### Key Performance Indicators (KPIs)
* **Uplink BLER**: Network operators should expect a small, transient increase in uplink Block Error Rate (BLER) during the first days following feature activation as the control loops converge network-wide.
* **Stabilization**: This transient BLER increase settles once the `pcTargetSinr` parameter is properly tuned.

# Cross-References
* [Parameters](parameters.md) — For details on the `pcTargetSinr` parameter and other power control configuration settings.
* [Feature Operation](feature-operation.md) — For details on how the closed-loop control loops function and converge.
