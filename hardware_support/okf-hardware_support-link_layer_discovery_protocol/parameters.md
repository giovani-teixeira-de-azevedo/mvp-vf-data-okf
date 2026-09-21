---
type: reference-table
resource: data/vodafone-mvp/raw/Link Layer Discovery Protocol.pdf#parameters
title: PARAMETERS
description: Configuration parameters for the Link Layer Discovery Protocol (LLDP)
  based on IEEE 802.1AB standard management objects.
tags:
- lldp
- ieee-802-1ab
- configuration
- parameters
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:44:35+00:00'
  source_sha256: 059128e564d4cb23
sources:
- title: Link Layer Discovery Protocol
  resource: data/vodafone-mvp/raw/Link Layer Discovery Protocol.pdf
---

This section defines the configuration parameters for the Link Layer Discovery Protocol (LLDP). The parameter set is aligned with IEEE 802.1AB standard management objects. 

Under normal operating conditions, the default values match the standard's recommendations and rarely require adjustment. The most operationally relevant settings are the per-port `adminStatus` and the optional TLV selection mask (`optionalTlvs`).

| Parameter | Description | Values | Datatype | Default |
| :--- | :--- | :--- | :--- | :--- |
| `adminStatus` | Per-port LLDP mode | DISABLED, TX_ONLY, RX_ONLY, TX_AND_RX | enum | TX_AND_RX |
| `txInterval` | Advertisement transmit interval | 5–3600 (s) | int32 | 30 |
| `txHoldMultiplier` | TTL = txInterval × this multiplier | 2–10 | int32 | 4 |
| `fastTxInterval` | Interval during fast-transmit burst | 1–5 (s) | int32 | 1 |
| `fastTxCount` | Frames in the fast-transmit burst | 1–8 | int32 | 4 |
| `optionalTlvs` | Bitmask of optional TLVs to send | SYS_NAME, SYS_DESC, MGMT_ADDR, MAX_FRAME, LAG | enum list | all |
| `maxNeighborsPerPort` | Retained neighbor entries per port | 1–4 | int32 | 1 |
| `notifyOnChange` | Emit notification on neighbor change | true, false | boolean | true |
