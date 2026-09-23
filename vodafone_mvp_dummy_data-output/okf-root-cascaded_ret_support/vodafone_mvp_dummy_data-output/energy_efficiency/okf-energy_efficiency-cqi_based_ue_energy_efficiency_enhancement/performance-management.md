---
type: concept
resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf#performance-management
title: PERFORMANCE MANAGEMENT
description: Covers proxy-based performance monitoring, key performance indicators,
  formulas, and cell-level counters for CQI-Based UE Energy Efficiency Enhancement.
tags:
- performance-management
- kpi
- counters
- cqi
- ue-energy-efficiency
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T14:17:56+00:00'
  source_sha256: cb94e7712d45a26b
sources:
- resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf
  title: CQI-Based UE Energy Efﬁciency Enhancement
---

Performance management for CQI-Based UE Energy Efficiency Enhancement evaluates how much UE active time the feature removes (the proxy for UE battery saving) and whether relaxation mechanisms degrade user throughput or link performance. Because UE battery level is not observable from the network, the monitoring strategy relies on comparing UE active-time-per-byte and HARQ retransmission KPIs against a two-week pre-activation baseline over the same hours of the day. All counters accumulate per cell over a 15-minute Result Output Period (ROP).

## Monitoring Guidelines

- **Active Time Efficiency**: The headline KPI; expect a 5–15% reduction after activation on cells with a healthy CQI spread.
- **Relaxed Population share**: Indicates how many UEs sit in poor conditions — values above 30% suggest a coverage problem that energy tuning cannot fix.
- **Retransmission Reduction**: Should be clearly positive for the relaxed class; if it is not, `mcsBackoff` is too small to matter or the low-CQI population is interference-limited rather than noise-limited.

## Key Performance Indicators (KPIs)

| KPI | Formula | Description |
| --- | --- | --- |
| Active Time Efficiency | `ctrUeActiveTime / ctrDlDataVolume` | UE active ms per MB delivered; lower is better |
| Race-to-Sleep Share | `ctrRtsBursts / ctrDlBursts × 100` | Share of DL bursts scheduled in compact mode (%) |
| Relaxed Population | `ctrRelaxedUeTime / ctrConnUeTime × 100` | Share of UE connected time spent in relaxed class (%) |
| Retransmission Reduction | `ctrHarqRetxRelaxed / ctrHarqTxRelaxed × 100` | HARQ retransmission rate within the relaxed class (%) |

## Counters

The performance counters separate the two UE classes so that each mechanism can be evaluated independently:

- `ctrClassTransitions` deserves attention: a high rate relative to connected users indicates CQI oscillation around the thresholds — widen `cqiClassHysteresis` or lengthen `cqiFilterTime`.
- `ctrUeActiveTime` counts slots in which the UE was scheduled or monitoring outside C-DRX sleep, aggregated over all UEs.

| Counter | Description | Range | Datatype |
| --- | --- | --- | --- |
| `ctrUeActiveTime` | Aggregated UE active time (ms) per ROP | 0–2³¹ | int64 |
| `ctrConnUeTime` | Aggregated UE RRC-connected time (ms) per ROP | 0–2³¹ | int64 |
| `ctrDlDataVolume` | Downlink data volume delivered (MB) per ROP | 0–2³¹ | int64 |
| `ctrDlBursts` | Downlink data bursts scheduled | 0–2³¹ | int64 |
| `ctrRtsBursts` | Bursts scheduled in race-to-sleep compact mode | 0–2³¹ | int64 |
| `ctrRelaxedUeTime` | Aggregated UE time in relaxed class (ms) | 0–2³¹ | int64 |
| `ctrHarqTxRelaxed` | HARQ initial transmissions to relaxed-class UEs | 0–2³¹ | int64 |
| `ctrHarqRetxRelaxed` | HARQ retransmissions to relaxed-class UEs | 0–2³¹ | int64 |
| `ctrClassTransitions` | UE transitions between CQI classes | 0–2³¹ | int64 |

# Cross-References

- [Parameters](parameters.md) — Feature parameters including `mcsBackoff`, `cqiClassHysteresis`, and `cqiFilterTime`.
- [Feature Operation](feature-operation.md) — CQI class transitions and scheduling operational details.
