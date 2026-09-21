---
type: concept
resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf#performance-management
title: PERFORMANCE MANAGEMENT
description: Outlines the performance management strategy, key performance indicators
  (KPIs), and performance counters for evaluating the CQI-Based UE Energy Efficiency
  Enhancement feature.
tags:
- telemetry
- kpi
- counters
- performance-monitoring
- energy-efficiency
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:38:46+00:00'
  source_sha256: cb94e7712d45a26b
sources:
- title: CQI-Based UE Energy Efﬁciency Enhancement
  resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf
---

This section outlines the performance management strategy, key performance indicators (KPIs), and telemetry counters used to assess and monitor the CQI-Based UE Energy Efficiency Enhancement feature.

## Monitoring Strategy

Since the UE battery level cannot be directly observed from the network side, the performance monitoring strategy relies on proxy-based evaluation:
*   **Proxy-Based Monitoring:** Compares UE active-time-per-byte and HARQ retransmission KPIs against a two-week pre-activation baseline over the same hours of the day.
*   **Aggregation:** All counters accumulate per cell over a 15-minute Reporting Period (ROP).

The strategy addresses two primary questions:
1.  How much UE active time the feature removes (serving as a proxy for UE battery savings).
2.  Whether the relaxation mechanisms cause any degradation to user throughput or link performance.

---

## Key Performance Indicators (KPIs)

*   **Active Time Efficiency:** This is the headline KPI. A 5–15% reduction is expected after activation on cells with a healthy CQI spread.
*   **Relaxed Population Share:** Indicates the percentage of UEs in poor channel conditions. Values above 30% suggest a coverage problem that cannot be resolved through energy tuning.
*   **Retransmission Reduction:** Should be clearly positive for the relaxed class. If it is not, then either `mcsBackoff` is too small to have an effect, or the low-CQI population is interference-limited rather than noise-limited.

| KPI | Formula | Description |
|---|---|---|
| **Active Time Efficiency** | `ctrUeActiveTime / ctrDlDataVolume` | UE active ms per MB delivered; lower is better. |
| **Race-to-Sleep Share** | `(ctrRtsBursts / ctrDlBursts) * 100` | Share of DL bursts scheduled in compact mode (%). |
| **Relaxed Population** | `(ctrRelaxedUeTime / ctrConnUeTime) * 100` | Share of UE connected time spent in relaxed class (%). |
| **Retransmission Reduction** | `(ctrHarqRetxRelaxed / ctrHarqTxRelaxed) * 100` | HARQ retransmission rate within the relaxed class (%). |

---

## Performance Counters

The performance counters separate UEs into classes to allow independent evaluation of each mechanism. 

### Key Observational Insights
*   **`ctrClassTransitions`:** A high transition rate relative to connected users indicates CQI oscillation around the thresholds. In such cases, consider widening `cqiClassHysteresis` or lengthening `cqiFilterTime`.
*   **`ctrUeActiveTime`:** Counts slots in which the UE was scheduled or monitoring outside C-DRX sleep, aggregated over all UEs.

| Counter | Description | Range | Datatype |
|---|---|---|---|
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

*   [Parameters](parameters.md) - Details on parameters like `mcsBackoff`, `cqiClassHysteresis`, and `cqiFilterTime`.
*   [Feature Operation](feature-operation.md) - Detailed operation of relaxed classes and race-to-sleep scheduling mechanisms.
