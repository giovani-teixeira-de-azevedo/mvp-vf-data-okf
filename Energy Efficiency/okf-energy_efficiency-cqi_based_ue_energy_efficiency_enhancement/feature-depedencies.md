---
type: concept
resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf#feature-depedencies
title: FEATURE DEPEDENCIES
description: Describes feature, hardware, and network dependencies as well as operational
  limitations.
tags:
- feature-dependencies
- hardware-dependencies
- network-dependencies
- limitations
- drx
- cqi
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-25T17:21:25+00:00'
  source_sha256: e62699b05496672b
sources:
- title: CQI-Based UE Energy Efﬁciency Enhancement
  resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf
---

The feature modifies scheduler behavior and CSI reporting configuration, so its dependencies concentrate on scheduling and DRX features. Review the list per cell before activation, particularly the interaction with latency-sensitive scheduling features.

## Feature Dependencies

- Requires a valid license key (FAK-31525) installed and `FeatureCtrl=CqiUeEnergyEff` set to `ACTIVATED`.
- Requires Connected Mode DRX to be activated on the cell; the battery gain is realized as additional C-DRX sleep time.
- Interworks with NR Service-Adaptive DRX: service-specific DRX profiles are respected, and compaction operates within the active time they define.
- The conservative MCS bias is suppressed for bearers handled by Latency-Prioritized Scheduling to avoid adding retransmission-avoidance delay margin to latency-critical traffic.

## Hardware Dependencies

- No dedicated hardware. Runs in the baseband scheduler on all supported baseband units.

## Network Dependencies

- None. The feature is cell-local and requires no core, transport, or neighbor coordination.

## Limitations

- The relaxed CSI reporting interval is applied only to UEs indicating support for the configured CSI report periodicities; legacy UEs keep their existing configuration.
- Scheduling compaction is not applied to UEs in MU-MIMO pairing candidacy, since compact wideband allocations reduce pairing opportunities.
- Gains are model-estimated on the network side; actual UE battery saving depends on UE implementation and cannot be directly measured by the gNodeB.
- Not applied to RedCap UEs handled by Ericsson Reduced Capability Enabler, which have their own reduced monitoring framework.

# Cross-References

- [FEATURE OVERVIEW](feature-overview.md)
- [PARAMETERS](parameters.md)
- [ACTIVATION PROCEDURE](activation-procedure.md)
