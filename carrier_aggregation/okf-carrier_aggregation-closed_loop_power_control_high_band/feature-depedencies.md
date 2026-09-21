---
type: concept
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#feature-depedencies
title: Feature Dependencies
description: Outlines the feature, hardware, network, and limitation dependencies
  for Closed-Loop Power Control High-Band.
tags:
- closed-loop-power-control
- high-band
- dependencies
- limitations
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:36:59+00:00'
  source_sha256: dc114d13756b9239
sources:
- title: Closed-Loop Power Control High-Band
  resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
---

The Closed-Loop Power Control High-Band feature operates within the gNodeB scheduler and physical layer control plane. It assumes a functioning high-band carrier baseline and interacts with other uplink features that manipulate power or scheduling. 

This section details the specific feature, hardware, and network dependencies, as well as the operational limitations that must be reviewed prior to deployment.

## Feature Dependencies

*   **Prerequisite Features**: Requires **Physical Layer High-Band** and **Scheduler High-Band** to be active on the node.
*   **Licensing and Configuration**: Requires a valid license key (`FAK-33011`) and the parameter `FeatureCtrl=ClosedLoopPcHighBand` set to `ACTIVATED`.
*   **Complementary Features**: Complements *NR Closed-Loop Power Control Low/Mid-Band*; both features can run on the same node, each governing its own frequency range.
*   **Interworking Features**: 
    *   **Coverage-Optimized Uplink Transmission High-Band**: When both features are active, the closed loop respects the coverage-optimized waveform switching points.
    *   **NR Uplink Carrier Aggregation High-Band**: Recommended to run together; per-carrier loop states prevent power imbalance across aggregated uplinks.

## Hardware Dependencies

*   **Radio Units**: Supported on all FR2 AAS (Active Antenna System) radio units of hardware generation R2 or later with per-beam received power reporting.
*   **Baseband Units**: Must have the high-band channel estimation package. Entry-level baseband variants are not supported.

## Network Dependencies

*   **Core and Transport Network**: No core network or transport dependencies; the control loop is strictly cell-local.
*   **UE Capability**: User Equipment (UEs) must support Transmit Power Control (TPC) accumulation for FR2 as per TS 38.213. Legacy UEs that ignore TPC on FR2 will automatically fall back to open-loop behavior.

## Limitations

*   **Controlled Channels**: The closed loop controls PUSCH and SRS. PRACH and Msg3 power remain open-loop by 3GPP design.
*   **Correction Range**: The maximum loop correction range is $\pm12$ dB relative to the open-loop operating point.
*   **Beam Switch Events**: During beam switch events, the loop state is reset to avoid applying corrections derived from the previous beam.
*   **TPC Mode**: Absolute TPC mode is not supported in this release; only accumulated mode is used.

# Cross-References

*   [Feature Overview](feature-overview.md) — For a high-level overview of the Closed-Loop Power Control High-Band feature.
*   [Feature Operation](feature-operation.md) — For details on loop initialization, convergence, and power adjustments.
*   [Parameters](parameters.md) — For configuring the activation and control parameters, including `FeatureCtrl=ClosedLoopPcHighBand`.
*   [Activation Procedure](activation-procedure.md) — Steps to license, configure, and activate the feature.
