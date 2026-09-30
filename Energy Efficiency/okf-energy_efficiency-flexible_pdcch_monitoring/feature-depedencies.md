---
type: concept
resource: data/vodafone-mvp/raw/Flexible PDCCH Monitoring.pdf#feature-depedencies
title: Feature Dependencies
description: Outlines feature, hardware, and network dependencies as well as operational
  limitations for Flexible PDCCH Monitoring.
tags:
- pdcch-monitoring
- feature-dependencies
- hardware-dependencies
- limitations
- c-drx
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-30T14:20:00+00:00'
  source_sha256: a6d5b31fa6584863
sources:
- title: Flexible PDCCH Monitoring
  resource: data/vodafone-mvp/raw/Flexible PDCCH Monitoring.pdf
---

This section details the feature, hardware, and network dependencies as well as operational limitations for Flexible PDCCH Monitoring. The feature configures UE monitoring behavior over RRC and steers it via DCI, so its dependencies center on UE capability and features that share control of the UE activity timeline.

## Feature Dependencies

- **License and Activation**: Requires a valid license key (`FAK-31545`) installed and `FeatureCtrl=FlexPdcchMonitoring` set to `ACTIVATED`.
- **Connected Mode DRX**: Interworks with Connected Mode DRX; monitoring adaptation operates within the DRX active time, and the `sssgSwitchTimer` should be set shorter than the DRX inactivity timer.
- **NR Service-Adaptive DRX**: Interworks with NR Service-Adaptive DRX; service profiles that demand tight latency (VoNR, low-latency 5QIs) automatically pin the UE to the dense monitoring group.
- **Latency-Prioritized Scheduling**: Bearers under Latency-Prioritized Scheduling exclude the UE from PDCCH skipping.
- **CQI-Based UE Energy Efficiency Enhancement**: Complements CQI-Based UE Energy Efficiency Enhancement; race-to-sleep bursts end with an immediate switch to sparse monitoring.

## Hardware Dependencies

- No dedicated network hardware. All supported baseband units.

## Network Dependencies

- None. The feature is cell-local.

## Limitations

- **UE Capability**: Requires UE support for search space set group switching and/or PDCCH skipping (3GPP Release 16/17 UE power saving capability); legacy UEs keep static monitoring and are unaffected.
- **Scheduling Latency**: Sparse monitoring adds up to one sparse period (default 4 slots, i.e. 2 ms at 30 kHz SCS) of scheduling latency for downlink data arriving in a quiet period.
- **Pending HARQ Retransmission**: PDCCH skipping is not applied while a HARQ retransmission is pending toward the UE.
- **Maximum Skip Duration**: Maximum configured skip duration is 40 ms; longer quiet periods are the domain of C-DRX.

# Cross-References

- [Feature Overview](feature-overview.md)
- [Activation Procedure](activation-procedure.md)
- [Parameters](parameters.md)
