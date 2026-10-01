---
type: concept
resource: data/vodafone-mvp/raw/Coverage-Optimized Uplink Transmission High-Band.pdf#feature-depedencies
title: Feature Dependencies
description: Describes the feature, hardware, and network dependencies, as well as
  limitations for Coverage-Optimized Uplink Transmission High-Band.
tags:
- dependencies
- hardware-requirements
- limitations
- high-band
- coverage-optimization
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T11:14:59+00:00'
  source_sha256: d7d99b19e71c73ba
sources:
- resource: data/vodafone-mvp/raw/Coverage-Optimized Uplink Transmission High-Band.pdf
  title: Coverage-Optimized Uplink Transmission High-Band
---

The Coverage-Optimized Uplink Transmission High-Band feature reshapes the uplink physical-layer configuration of edge UEs. Because of this, it interacts with scheduler, waveform, and coverage features on the same cell. This section details the feature, hardware, and network dependencies, as well as the operational limitations that must be reviewed per high-band cell before rollout.

## Feature Dependencies

* **Baseline Features:** Requires *Physical Layer High-Band* and *Scheduler High-Band* as baseline features on the cell.
* **Licensing and Activation:** Requires a valid license key (`FAK-33110`) installed and the parameter `FeatureCtrl=CovOptUlTxHighBand` set to `ACTIVATED`.
* **Waveform Interworking:** Interworks with *DFTS-OFDM Waveform in Uplink*. If that feature is active, waveform switching is delegated to it, and this feature only controls repetition and allocation adaptation.
* **Recommended Pairings:** Recommended together with *Coverage-Optimized Uplink Scheduling High-Band* and *Coverage Extension High-Band* for a consistent edge-of-cell strategy.
* **Mutual Exclusivity:** Mutually exclusive with *Optimized Uplink Peak Throughput High-Band* for the same UE at the same time. The scheduler arbitrates per UE, and peak-throughput mode is suppressed once coverage mode is entered.

## Hardware Dependencies

* **Radio Units:** Supported on all high-band AAS (Active Antenna System) radio units; there are no radio hardware restrictions.
* **Baseband Units:** Baseband unit generation B2 or later is required for PUSCH repetition combining at full cell capacity.

## Network Dependencies

* **Core Network:** None. The feature is cell-local, confined to the gNodeB and the Uu interface, and has no core network prerequisites.
* **UE Capabilities:** UEs must support transform precoding and PUSCH repetition Type A (which is mandatory for FR2 per TS 38.306 capability signaling). Non-supporting UEs receive only the MCS and allocation adaptations.

## Limitations

* **Uplink Capacity:** PUSCH repetition reduces uplink cell capacity when many UEs are simultaneously in coverage mode. The scheduler caps coverage-mode UEs at a configurable share of UL slots.
* **Random Access:** Not applied to 2-step RACH `msgA` transmissions.
* **Uplink Latency:** Uplink latency for edge UEs increases by up to 4 ms at maximum repetition ($K=8$) with a typical FR2 TDD pattern.
* **URLLC Exclusion:** URLLC 5QIs (82–85) are excluded from repetition adaptation to protect their latency budget.

# Cross-References

* [Feature Overview](feature-overview.md) — For an overview of the Coverage-Optimized Uplink Transmission High-Band feature.
* [Feature Operation](feature-operation.md) — For details on how repetition and allocation adaptation are performed.
* [Network Impact](network-impact.md) — For details on how this feature impacts cell capacity and latency.
* [Parameters](parameters.md) — For details on `FeatureCtrl` and other configurable parameters.
* [Activation Procedure](activation-procedure.md) — For step-by-step instructions on activating the feature and license.
