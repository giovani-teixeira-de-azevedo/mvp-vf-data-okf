---
type: concept
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#feature-depedencies
title: Feature Dependencies
description: Defines feature, hardware, network dependencies, and operational limitations
  for Closed-Loop Power Control High-Band.
tags:
- closed-loop power control
- high-band
- FR2
- feature dependencies
- hardware dependencies
- network dependencies
- limitations
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-25T11:05:48+00:00'
  source_sha256: dc114d13756b9239
sources:
- resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
  title: Closed-Loop Power Control High-Band
---

This section outlines the feature, hardware, and network dependencies, as well as operational limitations, for the Closed-Loop Power Control High-Band feature. The feature operates entirely within the gNodeB scheduler and physical layer control plane, assuming a functioning high-band carrier baseline.

## Overview

The feature operates entirely within the gNodeB scheduler and physical layer control plane, but it assumes a functioning high-band carrier baseline and interacts with other uplink features that manipulate power or scheduling. Review the following dependencies per node before rollout.

## Feature Dependencies

- **Node Prerequisites:** Requires Physical Layer High-Band and Scheduler High-Band to be active on the node.
- **License and Control Parameter:** Requires a valid license key (`FAK-33011`) and `FeatureCtrl=ClosedLoopPcHighBand` set to `ACTIVATED`.
- **Low/Mid-Band Complement:** Complements NR Closed-Loop Power Control Low/Mid-Band; both can run on the same node, each governing its own frequency range.
- **Coverage Optimization Interworking:** Interworks with Coverage-Optimized Uplink Transmission High-Band: when both are active, the closed loop respects the coverage-optimized waveform switching points.
- **Carrier Aggregation Recommendation:** Recommended together with NR Uplink Carrier Aggregation High-Band, where per-carrier loop states prevent power imbalance across aggregated uplinks.

## Hardware Dependencies

- **Radio Units:** Supported on all FR2 AAS radio units of hardware generation R2 or later with per-beam received power reporting.
- **Baseband Units:** Baseband units must have the high-band channel estimation package; entry-level baseband variants are not supported.

## Network Dependencies

- **Core & Transport:** No core network or transport dependencies; the control loop is cell-local.
- **UE Requirements:** UEs must support TPC accumulation for FR2 as per TS 38.213; legacy UEs that ignore TPC on FR2 fall back to open-loop behavior automatically.

## Limitations

- **Channel Scope:** The closed loop controls PUSCH and SRS; PRACH and Msg3 power remain open-loop by 3GPP design.
- **Correction Range:** Maximum loop correction range is ±12 dB relative to the open-loop operating point.
- **Beam Switching:** During beam switch events, the loop state is reset to avoid applying corrections derived from the previous beam.
- **TPC Mode:** Absolute TPC mode is not supported in this release; only accumulated mode is used.

# Cross-References

- [Feature Overview](feature-overview.md)
- [Parameters](parameters.md)
- [Activation Procedure](activation-procedure.md)
