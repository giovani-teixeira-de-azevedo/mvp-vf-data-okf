---
type: concept
resource: data/vodafone-mvp/raw/NR Massive MIMO Sleep Mode.pdf#feature-depedencies
title: Feature Dependencies
description: Preconditions, software/hardware/network dependencies, interworking constraints,
  and operational limitations for NR Massive MIMO Sleep Mode.
tags:
- massive-mimo
- sleep-mode
- feature-dependencies
- hardware-dependencies
- network-dependencies
- limitations
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-25T16:09:29+00:00'
  source_sha256: b2e6396b4c35c66c
sources:
- resource: data/vodafone-mvp/raw/NR Massive MIMO Sleep Mode.pdf
  title: NR Massive MIMO Sleep Mode
---

This section outlines the software, hardware, network dependencies, and functional limitations required before deploying NR Massive MIMO Sleep Mode on a cell.

## Feature Dependencies

- Requires the Massive MIMO Mid-Band (or Massive MIMO High-Band) baseline feature to be activated on the cell.
- Requires a valid license key (`FAK-31240`) installed and `FeatureCtrl=MassiveMimoSleep` set to `ACTIVATED`.
- Interworks with NR Massive MIMO Eco Mode; if both are active, Eco Mode power reduction is applied only to the branches that remain active during sleep.
- Mutually exclusive with NR Flexible Cell Shaping High-Band on the same cell, since both manipulate the common channel beam configuration.

## Hardware Dependencies

- Supported only on AAS radio units with 32 or 64 transceiver branches that support per-branch power gating (radio hardware generation R2 or later).
- Not supported on remote radio units driving passive antennas.

## Network Dependencies

- No core network or transport dependencies. The feature is cell-local.
- In Multi-Operator RAN (MORAN/MOCN) deployments, the sleep decision considers the aggregated load of all sharing operators; per-operator thresholds are not supported.

## Limitations

- Sleep transitions are blocked while an emergency call or MPS-prioritized bearer is active in the cell.
- Uplink SRS-based beamforming accuracy is reduced in Deep Sleep; UEs relying on reciprocity-based precoding fall back to codebook-based transmission.
- Cell-edge downlink throughput can degrade by up to 15% in Deep Sleep due to the reduced beamforming gain.
- Maximum of one sleep-capable carrier per radio unit may be configured for Deep Sleep.

# Cross-References

- [Activation Procedure](activation-procedure.md)
