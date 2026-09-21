---
type: concept
resource: data/vodafone-mvp/raw/NR Massive MIMO Sleep Mode.pdf#feature-depedencies
title: Feature Dependencies
description: Outlines the feature, hardware, and network dependencies, as well as
  operational limitations, for NR Massive MIMO Sleep Mode.
tags:
- massive-mimo
- sleep-mode
- dependencies
- limitations
- hardware-requirements
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:42:05+00:00'
  source_sha256: b2e6396b4c35c66c
sources:
- resource: data/vodafone-mvp/raw/NR Massive MIMO Sleep Mode.pdf
  title: NR Massive MIMO Sleep Mode
---

This section outlines the software, hardware, and network preconditions, as well as operational limitations, that must be verified prior to activating the NR Massive MIMO Sleep Mode feature. Because sleep mode manipulates the radio branch configuration and the common channel beam grid, rollout planning requires a cell-by-cell review of these dependencies.

## Feature Dependencies

*   **Baseline Feature:** Requires the Massive MIMO Mid-Band (or Massive MIMO High-Band) baseline feature to be active on the cell.
*   **Licensing and Configuration:** Requires a valid license key (`FAK-31240`) installed and parameter `FeatureCtrl` set to include `MassiveMimoSleep` as `ACTIVATED`.
*   **NR Massive MIMO Eco Mode:** Interworks with this feature. If both are active, Eco Mode power reduction is applied only to the transceiver branches that remain active during sleep.
*   **NR Flexible Cell Shaping High-Band:** Mutually exclusive on the same cell, as both features manipulate the common channel beam configuration.

## Hardware Dependencies

*   **Supported Radio Units:** Supported only on Active Antenna System (AAS) radio units with 32 or 64 transceiver branches that support per-branch power gating (radio hardware generation R2 or later).
*   **Non-Supported Hardware:** Not supported on remote radio units (RRUs) driving passive antennas.

## Network Dependencies

*   **Transport and Core:** No core network or transport dependencies. The feature is completely cell-local.
*   **Multi-Operator RAN (MORAN/MOCN):** In shared deployments, the sleep decision considers the aggregated load across all sharing operators. Per-operator thresholds are not supported.

## Limitations

*   **Emergency & Priority Calls:** Sleep transitions are blocked while an emergency call or Multimedia Priority Service (MPS)-prioritized bearer is active in the cell.
*   **Uplink Beamforming:** Uplink SRS-based beamforming accuracy is reduced in Deep Sleep. User Equipments (UEs) relying on reciprocity-based precoding fall back to codebook-based transmission.
*   **Cell-Edge Throughput:** Cell-edge downlink throughput can degrade by up to 15% in Deep Sleep due to the reduced beamforming gain.
*   **Carrier Limit:** A maximum of one sleep-capable carrier per radio unit may be configured for Deep Sleep.

# Cross-References

*   [Feature Overview](feature-overview.md) — For a high-level overview of the feature's purposes and behavior.
*   [Feature Operation](feature-operation.md) — For details on sleep states and operational transitions.
*   [Parameters](parameters.md) — Details on `FeatureCtrl` and related configuration settings.
*   [Activation Procedure](activation-procedure.md) — Steps for activating and configuring the sleep feature.
