---
type: concept
resource: data/vodafone-mvp/raw/EPS Fallback for IMS Voice.pdf#feature-operation
title: Feature Operation
description: Detailed operational logic and decision flow for EPS Fallback for IMS
  Voice in the gNodeB.
tags:
- EPS Fallback
- IMS Voice
- gNodeB
- Handover
- Redirection
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T10:15:03+00:00'
  source_sha256: de0ad778f62275f9
sources:
- resource: data/vodafone-mvp/raw/EPS Fallback for IMS Voice.pdf
  title: EPS Fallback for IMS Voice
---

This section describes the operational logic and decision-making process of the EPS Fallback for IMS Voice feature within the gNodeB. The operation is executed per NR cell with per-call decision logic.

## Fallback Decision Logic

The fallback procedure is initiated upon the reception of a **PDU Session Modification** carrying a **5QI 1 GBR flow**. Upon receiving this request, the gNodeB evaluates the fallback decision based on the configured `voiceFallbackMode` parameter:

*   **`FALLBACK_ALWAYS`**: Fallback is triggered unconditionally.
*   **`FALLBACK_IF_NO_VONR`**: Fallback is triggered only when Voice over NR (VoNR) is not permitted for the UE. VoNR is considered not permitted under any of the following conditions:
    *   The UE lacks VoNR capability.
    *   The coverage is below the threshold defined by `vonrRsrpThr`.
    *   VoNR is disabled on the serving NR cell.

Once the fallback decision is made, the gNodeB rejects the 5QI 1 GBR flow with the standardized fallback cause. This ensures that the Session Management Function (SMF) retains the voice flow context for subsequent re-establishment on the Evolved Packet System (EPS).

## Measurement and Target Selection

If measurement-based fallback is enabled (`fallbackMeasEnabled` is set to `true`), the gNodeB configures the UE with an **Event B1 measurement** (as specified in TS 38.331). 

The measurement configuration uses the following parameters:
*   **Frequencies**: Configured on the frequencies specified in `lteTargetFreqList`.
*   **Threshold**: Defined by `b1ThresholdRsrp`.
*   **Timer**: Guarded by `fallbackMeasTimer`.

The fallback execution is triggered by one of the following events:
1.  **Measurement Report Reception**: The UE reports a suitable LTE target frequency.
2.  **Timer Expiry**: If the `fallbackMeasTimer` expires before a report is received, the gNodeB blindly selects the highest-priority configured target frequency from the list.

## Fallback Execution Methods

The gNodeB executes the fallback using the method selected by the `fallbackMethod` parameter:

### Handover-Based Fallback
The gNodeB initiates an inter-system handover. The `NG Handover Required` message is sent to the Core Network, carrying the selected E-UTRAN target cell information.

### Redirection-Based Fallback
The gNodeB releases the RRC connection. The `RRCRelease` message is sent to the UE carrying `redirectedCarrierInfo` containing the chosen E-UTRAN Absolute Radio Frequency Channel Number (EARFCN).

# Cross-References

*   [Feature Overview](feature-overview.md) — For an overview of the EPS Fallback feature.
*   [Parameters](parameters.md) — For details on parameters such as `voiceFallbackMode`, `vonrRsrpThr`, `fallbackMeasEnabled`, `lteTargetFreqList`, `b1ThresholdRsrp`, `fallbackMeasTimer`, and `fallbackMethod`.
*   [Activation Procedure](activation-procedure.md) — For steps to enable this feature.
*   [Deactivation Procedure](deactivation-procedure.md) — For steps to disable this feature.
