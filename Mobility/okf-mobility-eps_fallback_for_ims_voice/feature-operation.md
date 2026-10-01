---
type: concept
resource: data/vodafone-mvp/raw/EPS Fallback for IMS Voice.pdf#feature-operation
title: Feature Operation
description: Describes the per-cell and per-call decision logic, measurement configuration,
  and execution methods for EPS Fallback.
tags:
- EPS Fallback
- gNodeB
- VoNR
- Handover
- Redirection
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T11:01:24+00:00'
  source_sha256: de0ad778f62275f9
sources:
- resource: data/vodafone-mvp/raw/EPS Fallback for IMS Voice.pdf
  title: EPS Fallback for IMS Voice
---

This section describes the operational logic of the EPS Fallback feature, which is executed per NR cell with per-call decision logic. It details the decision-making process, measurement configuration, and execution methods for fallback to EPS.

## Fallback Decision Logic

When the gNodeB receives a **PDU Session Modification** carrying a **5QI 1 GBR flow**, it evaluates the fallback decision based on the configured `voiceFallbackMode`:

*   **`FALLBACK_ALWAYS`**: Fallback is triggered unconditionally.
*   **`FALLBACK_IF_NO_VONR`**: Fallback is triggered only when Voice over NR (VoNR) is not permitted for the UE. VoNR is considered not permitted if any of the following conditions are met:
    *   The UE lacks VoNR capability.
    *   The coverage is below the `vonrRsrpThr` threshold.
    *   VoNR is disabled on the cell.

Upon deciding to fall back, the gNodeB rejects the 5QI 1 GBR flow with a standardized fallback cause. This ensures that the Session Management Function (SMF) retains the voice flow context for subsequent re-establishment on the Evolved Packet System (EPS).

## Measurement and Target Selection

If measurement-based fallback is enabled (`fallbackMeasEnabled` is `true`), the gNodeB configures an **Event B1 measurement** (as specified in TS 38.331) on the frequencies defined in `lteTargetFreqList`. 

The measurement configuration uses the following parameters:
*   **Threshold**: `b1ThresholdRsrp`
*   **Guard Timer**: `fallbackMeasTimer`

The fallback execution is triggered by one of two events:
1.  **Measurement Report Reception**: The gNodeB receives a B1 measurement report from the UE.
2.  **Timer Expiry**: If the `fallbackMeasTimer` expires before a report is received, the gNodeB blindly selects the highest-priority configured target frequency.

## Fallback Execution Methods

Once the target is determined, the gNodeB executes the fallback using the method configured in `fallbackMethod`:

### Handover-Based Fallback
The gNodeB initiates an NG-based handover. The `NG Handover Required` message is sent to the Core Network carrying the selected E-UTRAN target cell information.

### Redirect-Based Fallback
The gNodeB releases the RRC connection. The `RRCRelease` message is sent to the UE carrying `redirectedCarrierInfo` populated with the chosen E-UTRAN Absolute Radio Frequency Channel Number (EARFCN).

# Cross-References

*   [Feature Overview](feature-overview.md) — For an overview of the EPS Fallback feature.
*   [Parameters](parameters.md) — For details on the configuration parameters such as `voiceFallbackMode`, `fallbackMeasEnabled`, and `fallbackMethod`.
*   [Activation Procedure](activation-procedure.md) — For instructions on enabling the feature.
