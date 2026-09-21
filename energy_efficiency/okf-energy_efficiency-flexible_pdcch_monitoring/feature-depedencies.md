---
type: concept
resource: data/vodafone-mvp/raw/Flexible PDCCH Monitoring.pdf#feature-depedencies
title: Feature Dependencies
description: Details the license, hardware, network, and interworking dependencies,
  as well as limitations of the Flexible PDCCH Monitoring feature.
tags:
- PDCCH
- C-DRX
- UE Capability
- Feature Dependencies
- License
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:41:37+00:00'
  source_sha256: a6d5b31fa6584863
sources:
- resource: data/vodafone-mvp/raw/Flexible PDCCH Monitoring.pdf
  title: Flexible PDCCH Monitoring
---

The Flexible PDCCH Monitoring feature configures User Equipment (UE) monitoring behavior over Radio Resource Control (RRC) and steers it via Downlink Control Information (DCI). Consequently, its dependencies and interworking requirements center on UE capability and on other features that share control of the UE activity timeline.

## Feature Dependencies

*   **License and Configuration:** Requires a valid license key (`FAK-31545`) installed and the parameter `FeatureCtrl=FlexPdcchMonitoring` set to `ACTIVATED`.
*   **Connected Mode DRX (C-DRX):** Interworks with Connected Mode DRX. The monitoring adaptation operates within the DRX active time, and the `sssgSwitchTimer` should be set shorter than the DRX inactivity timer.
*   **NR Service-Adaptive DRX:** Interworks with NR Service-Adaptive DRX. Service profiles that demand tight latency (such as VoNR or low-latency 5QIs) automatically pin the UE to the dense monitoring group.
*   **Latency-Prioritized Scheduling:** Bearers under Latency-Prioritized Scheduling exclude the UE from PDCCH skipping.
*   **CQI-Based UE Energy Efficiency Enhancement:** Complements this feature. Race-to-sleep bursts end with an immediate switch to sparse monitoring.

## Hardware Dependencies

*   No dedicated network hardware is required.
*   Supported on all baseband units.

## Network Dependencies

*   None. The feature is cell-local.

## Limitations

*   **UE Capability:** Requires UE support for search space set group (SSSG) switching and/or PDCCH skipping (3GPP Release 16/17 UE power saving capability). Legacy UEs keep static monitoring and are unaffected.
*   **Scheduling Latency:** Sparse monitoring adds up to one sparse period of scheduling latency for downlink data arriving in a quiet period. The default is 4 slots, which corresponds to 2 ms at 30 kHz SCS (Subcarrier Spacing).
*   **HARQ Retransmission:** PDCCH skipping is not applied while a HARQ (Hybrid Automatic Repeat Request) retransmission is pending toward the UE.
*   **Skip Duration:** The maximum configured skip duration is 40 ms. Longer quiet periods are the domain of C-DRX.

# Cross-References

*   [Feature Overview](feature-overview.md)
*   [Feature Operation](feature-operation.md)
*   [Parameters](parameters.md)
*   [Activation Procedure](activation-procedure.md)
