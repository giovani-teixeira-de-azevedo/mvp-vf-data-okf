---
type: concept
resource: data/vodafone-mvp/raw/Energy-Optimized Power Allocation.pdf#feature-depedencies
title: Feature Dependencies and Limitations
description: Details license requirements, feature interworkings, hardware and network
  dependencies, and functional limitations for Energy-Optimized Power Allocation.
tags:
- dependencies
- hardware
- limitations
- interworking
- energy-saving
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:39:20+00:00'
  source_sha256: 13b896b500ba3e60
sources:
- resource: data/vodafone-mvp/raw/Energy-Optimized Power Allocation.pdf
  title: Energy-Optimized Power Allocation
---

The **Energy-Optimized Power Allocation** feature operates inside downlink link adaptation and power control. Consequently, its interactions are primarily with power- and interference-related features. In particular, any Electromagnetic Field (EMF) or power-lock features that manipulate transmit power must be checked on a per-cell basis.

## Feature Dependencies

* **License & Activation**: Requires a valid license key **FAK-31530** installed and the parameter `FeatureCtrl=EnergyOptPowerAlloc` set to `ACTIVATED`.
* **Modulation-Aware Power Control**: When both features are active, modulation-specific power offsets are applied first, and the energy headroom reduction is applied on top, subject to a combined floor.
* **EMF Power Lock Mid-Band**: Fully compatible. The EMF lock defines the upper power bound; the Energy-Optimized Power Allocation feature only reduces power below this bound.
* **NR Massive MIMO Eco Mode**: Interworks on cells where Eco Mode already caps total power. In this case, the headroom evaluation utilizes the capped power as reference.

## Hardware Dependencies

* **Radio Units**: Supported on all radio unit generations. However, the additional Power Amplifier (PA) bias saving component requires radio hardware generation **R2 or later** with fast bias adaptation support.
* **Baseband**: No baseband hardware dependency.

## Network Dependencies

* **Local Operation**: None. The feature is cell-local. Reduced PDSCH power incidentally lowers inter-cell interference, which neighbor cells observe as a small SINR improvement.

## Limitations

* **Exempt Allocations**: Power reduction is not applied to:
  * Allocations carrying System Information Blocks (SIB)
  * Paging
  * Random Access Response (RAR)
  * Msg4
  * Retransmissions
* **MU-MIMO**: Not applied to UEs in Multi-User MIMO (MU-MIMO) paired transmissions, where per-layer power balance is managed by the pairing algorithm.
* **Maximum Reduction**: Capped at **6 dB** regardless of configuration to bound CQI-mismatch risk (since the UE measures Channel State Information Reference Signal (CSI-RS) at full power, large PDSCH offsets degrade the accuracy of CQI-to-PDSCH mapping).
* **Traffic Dependency**: Savings are traffic-dependent; an empty cell saves nothing through this feature (idle-slot savings are handled by features like **NR Micro Sleep Tx**).

# Cross-References

* [Feature Overview](feature-overview.md)
* [Feature Operation](feature-operation.md)
* [Parameters](parameters.md)
* [Activation Procedure](activation-procedure.md)
