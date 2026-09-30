---
type: concept
resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf#performance-management
title: Performance Management
description: Monitors UE battery saving proxies, throughput impact, KPIs, and performance
  counters for CQI-based UE energy efficiency enhancement.
tags:
- performance-management
- kpi
- counters
- cqi
- energy-efficiency
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-25T17:14:54+00:00'
  source_sha256: cb94e7712d45a26b
sources:
- resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf
  title: CQI-Based UE Energy Efﬁciency Enhancement
---

Performance management evaluates how much UE active time is removed as a proxy for UE battery saving, and monitors whether relaxation mechanisms degrade user throughput or link performance.

Because UE battery level is not directly observable from the network, performance monitoring uses a proxy-based strategy: UE active-time-per-byte and HARQ retransmission KPIs are compared against a two-week pre-activation baseline over identical hours of the day. All performance counters accumulate per cell over a 15-minute Result Output Period (ROP).

## Key Performance Indicators (KPIs)

- **Active Time Efficiency**: The headline KPI, expected to show a 5–15% reduction after feature activation on cells with a healthy CQI spread.
- **Relaxed Population**: Indicates the share of UEs sitting in poor conditions. Values exceeding 30% suggest a coverage problem that cannot be resolved through energy tuning.
- **Retransmission Reduction**: Should be clearly positive for the relaxed class. A non-positive value indicates that `mcsBackoff` is too small to have an effect or that the low-CQI population is interference-limited rather than noise-limited.

| KPI | Formula | Description |
| :--- | :--- | :--- |
| Active Time Efficiency | `ctrUeActiveTime / ctrDlDataVolume` | UE active ms per MB delivered; lower is better |
| Race-to-Sleep Share | `ctrRtsBursts / ctrDlBursts × 100` | Share of DL bursts scheduled in compact mode (%) |
| Relaxed Population | `ctrRelaxedUeTime / ctrConnUeTime × 100` | Share of UE connected time spent in relaxed class (%) |
| Retransmission Reduction | `ctrHarqRetxRelaxed / ctrHarqTxRelaxed × 100` | HARQ retransmission rate within the relaxed class (%) |

## Performance Counters

Counters separate the two UE classes to allow independent evaluation of each mechanism:

- `ctrClassTransitions`: A high transition rate relative to connected users indicates CQI oscillation around thresholds, signaling a need to widen `cqiClassHysteresis` or lengthen `cqiFilterTime`.
- `ctrUeActiveTime`: Counts slots in which the UE was scheduled or monitoring outside C-DRX sleep, aggregated over all UEs.

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
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

- [Parameters](parameters.md)
