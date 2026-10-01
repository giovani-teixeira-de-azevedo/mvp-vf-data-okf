---
type: reference-table
resource: data/vodafone-mvp/raw/EPS Fallback for IMS Voice.pdf#parameters
title: Parameters
description: Configuration parameters per NR cell for EPS Fallback for IMS Voice,
  including fallback modes, methods, and measurement thresholds.
tags:
- parameters
- configuration
- EPS Fallback
- VoLTE
- VoNR
- LTE
- NR
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T11:01:30+00:00'
  source_sha256: 79acb118bdc51c72
sources:
- title: EPS Fallback for IMS Voice
  resource: data/vodafone-mvp/raw/EPS Fallback for IMS Voice.pdf
---

This section defines the configuration parameters for EPS Fallback for IMS Voice, which are configured per NR cell. The parameters control the fallback decision policy, transfer methods, measurement configurations, and supervision timers.

Parameters are configured per NR cell. The method choice and the measurement configuration are the primary tuning surface: prefer `HANDOVER` where N26 and neighbor relations are mature, `REDIRECT` elsewhere, and keep measurements enabled in any multi-layer LTE market.

| Parameter | Description | Values | Datatype | Default |
| :--- | :--- | :--- | :--- | :--- |
| `voiceFallbackMode` | Fallback decision policy for 5QI 1 flows | `DISABLED`, `FALLBACK_ALWAYS`, `FALLBACK_IF_NO_VONR` | enum | `DISABLED` |
| `fallbackMethod` | Transfer method toward EPS | `HANDOVER`, `REDIRECT`, `HANDOVER_WITH_REDIRECT_FALLBACK` | enum | `REDIRECT` |
| `fallbackMeasEnabled` | Enables B1 measurement-based target selection | `true`, `false` | boolean | `true` |
| `lteTargetFreqList` | Candidate LTE EARFCNs for fallback | list of 1–8 EARFCNs | list | empty |
| `b1ThresholdRsrp` | RSRP threshold for event B1 on LTE targets | -140 to -44 (dBm) | int32 | -118 |
| `fallbackMeasTimer` | Max wait for B1 report before blind fallback | 200 to 3000 (ms) | int32 | 800 |
| `vonrRsrpThr` | NR RSRP below which VoNR is not attempted (`FALLBACK_IF_NO_VONR`) | -140 to -44 (dBm) | int32 | -110 |
| `fallbackGuardTimer` | Supervision of the overall fallback procedure | 1 to 10 (s) | int32 | 4 |

# Cross-References

* [Feature Operation](feature-operation.md)
* [Activation Procedure](activation-procedure.md)
* [Deactivation Procedure](deactivation-procedure.md)
