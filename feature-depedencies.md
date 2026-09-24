---
type: concept
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#feature-depedencies
title: Feature Dependencies
description: Outlines feature, hardware, and network dependencies as well as operating
  limitations for Closed-Loop Power Control High-Band.
tags:
- dependencies
- hardware
- limitations
- high-band
- power-control
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-24T14:32:22+00:00'
  source_sha256: dc114d13756b9239
sources:
- resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
  title: Closed-Loop Power Control High-Band
---

This section outlines the feature, hardware, and network dependencies as well as operational limitations for the Closed-Loop Power Control High-Band feature in the gNodeB scheduler and physical layer control plane.

## Overview

The feature operates entirely within the gNodeB scheduler and physical layer control plane, assuming a functioning high-band carrier baseline and interacting with other uplink features that manipulate power or scheduling.

## Dependencies

### Feature Dependencies

- Requires **Physical Layer High-Band** and **Scheduler High-Band** to be active on the node.
- Requires a valid license key (`FAK-33011`) and parameter setting `FeatureCtrl=ClosedLoopPcHighBand` set to `ACTIVATED`.
- Complements **NR Closed-Loop Power Control Low/Mid-Band**; both can run on the same node, each governing its own frequency range.
- Interworks with **Coverage-Optimized Uplink Transmission High-Band**: when both are active, the closed loop respects the coverage-optimized waveform switching points.
- Recommended together with **NR Uplink Carrier Aggregation High-Band**, where per-carrier loop states prevent power imbalance across aggregated uplinks.

### Hardware Dependencies

- Supported on all FR2 AAS radio units of hardware generation R2 or later with per-beam received power reporting.
- Baseband units must have the high-band channel estimation package; entry-level baseband variants are not supported.

### Network Dependencies

- No core network or transport dependencies; the control loop is cell-local.
- UEs must support TPC accumulation for FR2 as per TS 38.213; legacy UEs that ignore TPC on FR2 fall back to open-loop behavior automatically.

## Limitations

- The closed loop controls PUSCH and SRS; PRACH and Msg3 power remain open-loop by 3GPP design.
- Maximum loop correction range is ±12 dB relative to the open-loop operating point.
- During beam switch events, the loop state is reset to avoid applying corrections derived from the previous beam.
- Absolute TPC mode is not supported in this release; only accumulated mode is used.

# Cross-References

- [Parameters](parameters.md) – Parameter `FeatureCtrl=ClosedLoopPcHighBand` configuration.
- [Activation Procedure](activation-procedure.md) – Procedure for activating the feature on the node.
