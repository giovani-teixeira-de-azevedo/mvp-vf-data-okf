---
type: concept
resource: data/vodafone-mvp/raw/Energy-Optimized Power Allocation.pdf#feature-depedencies
title: FEATURE DEPEDENCIES
description: Feature, hardware, network dependencies, and functional limitations for
  Energy-Optimized Power Allocation.
tags:
- power-allocation
- dependencies
- hardware-requirements
- limitations
- license
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-25T15:03:45+00:00'
  source_sha256: 13b896b500ba3e60
sources:
- resource: data/vodafone-mvp/raw/Energy-Optimized Power Allocation.pdf
  title: Energy-Optimized Power Allocation
---

This section outlines the feature, hardware, and network dependencies, along with functional limitations, for Energy-Optimized Power Allocation within downlink link adaptation and power control.

The feature operates inside downlink link adaptation and power control, interacting with power- and interference-related features. Cell configurations should be checked, particularly for EMF or power-lock features that manipulate transmit power.

## Feature Dependencies

- **License and Control Parameter:** Requires a valid license key (`FAK-31530`) installed and `FeatureCtrl=EnergyOptPowerAlloc` set to `ACTIVATED`.
- **Modulation-Aware Power Control:** Interworks with Modulation-Aware Power Control. When both features are active, modulation-specific power offsets are applied first, followed by the energy headroom reduction on top, bounded by a combined floor.
- **EMF Power Lock Mid-Band:** Compatible with EMF Power Lock Mid-Band. The EMF lock defines the upper power bound, while Energy-Optimized Power Allocation only reduces transmit power below this limit.
- **NR Massive MIMO Eco Mode:** Interworks with NR Massive MIMO Eco Mode. On cells where Eco Mode caps total power, headroom evaluation uses the capped power as a reference.
- **Software Upgrade & Modem Restart:** Following a software upgrade, a restart of the modem may be required for newly updated software images and feature capabilities to take effect. For step-by-step activation, see [ACTIVATION PROCEDURE](activation-procedure.md).

## Hardware Dependencies

- **Radio Hardware:** Supported on all radio unit generations. The additional PA-bias saving component requires radio hardware generation R2 or later with fast bias adaptation.
- **Baseband Hardware:** No baseband hardware dependency.

## Network Dependencies

- **Network Scope:** None; the feature is cell-local. Reduced PDSCH power incidentally lowers inter-cell interference, which neighboring cells observe as a small SINR improvement.

## Limitations

- **Control & Common Channels:** Power reduction is not applied to allocations carrying SIB, paging, RAR, or Msg4, nor to retransmissions.
- **MU-MIMO:** Power reduction is not applied to UEs in MU-MIMO paired transmissions, where per-layer power balance is managed by the pairing algorithm.
- **Maximum Reduction Cap:** Maximum reduction is capped at 6 dB regardless of configuration to bound the CQI-mismatch risk (since the UE measures CSI-RS at full power, large PDSCH offsets degrade CQI-to-PDSCH accuracy).
- **Traffic Dependence:** Energy savings depend on traffic; an empty cell saves no power through this feature (refer to NR Micro Sleep Tx for idle-slot savings).

# Cross-References

- [ACTIVATION PROCEDURE](activation-procedure.md) — Covers procedures for enabling feature controls and handling configuration updates.
- [FEATURE OPERATION](feature-operation.md) — Details the downlink link adaptation and power allocation mechanism.
- [PARAMETERS](parameters.md) — Lists configuration parameters including `FeatureCtrl`.
