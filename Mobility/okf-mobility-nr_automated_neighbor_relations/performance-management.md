---
type: concept
resource: data/vodafone-mvp/raw/NR Automated Neighbor Relations.pdf#performance-management
title: PERFORMANCE MANAGEMENT
description: Performance management guidelines, KPIs, and counters for NR Automated
  Neighbor Relations (ANR).
tags:
- ANR
- Performance Management
- KPIs
- Counters
- NR
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T16:53:49+00:00'
  source_sha256: 50b13d01b5091204
sources:
- resource: data/vodafone-mvp/raw/NR Automated Neighbor Relations.pdf
  title: NR Automated Neighbor Relations
---

This section describes the performance management framework for NR Automated Neighbor Relations (ANR). It outlines the key performance indicators (KPIs), counter definitions, and troubleshooting guidelines used to monitor discovery pace, table health, and operational costs.

Performance management for ANR answers three core operational questions:
1. **Is discovery keeping pace with the network?** (e.g., new relations created, CGI resolution success)
2. **Is the neighbor relation table healthy?** (e.g., removals, table size, chronic-failure flags)
3. **Is the signaling cost bounded?** (e.g., CGI order volume)

### Monitoring Guidelines
* **Review Frequency:** During network growth phases, KPIs should be reviewed weekly. In a steady-state network, a monthly review combined with alarm-driven attention is sufficient.
* **Baseline Comparison:** Performance is baselined against the handover success rate observed before ANR-discovered relations became active.
* **Accumulation Interval:** All performance counters accumulate per cell over a 15-minute Reporting Period (ROP).

---

## Key Performance Indicators (KPIs)

The KPIs are derived from the underlying performance counters. 

* **CGI Resolution Success:** This metric should ideally exceed 90%. Persistently lower values indicate that the `cgiReportTimer` is configured too short for the local DRX configuration, or there is a significant population of non-capable UEs.
* **Relation Churn:** In a stable network, relation churn should approach zero. Sustained churn indicates that the removal timer is deleting relations that are subsequently re-discovered; in this scenario, the `relationRemovalTime` parameter should be increased.
* **Discovery Rate:** This metric is naturally bursty (influenced by new site integrations or seasonal traffic changes) and should be interpreted in the context of network integration activity rather than in isolation.

### KPI Formulas

| KPI | Formula | Description |
| :--- | :--- | :--- |
| **CGI Resolution Success (%)** | `ctrCgiReportOk / ctrCgiOrdered * 100` | Share of CGI orders returning a decoded CGI. |
| **Discovery Rate** | `ctrRelationCreated / (ROP count per day)` | New relations created per day. |
| **Relation Churn (%)** | `ctrRelationRemoved / ctrRelationCreated * 100` | Removed-to-created ratio over the trending window. |
| **Auto-Xn Success (%)** | `ctrXnSetupOk / ctrXnSetupAttempt * 100` | Xn establishments succeeding after discovery. |

---

## Performance Counters

Specific counters provide deeper diagnostic insight:
* **`ctrCgiReportFail`:** If failures cluster on specific target ARFCNs, it typically indicates that the target layer's SIB1 periodicity or coverage makes autonomous-gap reading marginal, rather than indicating a UE capability issue.
* **`ctrRelationHoFailFlagged`:** This counter tracks relations that cross the chronic handover failure threshold. It should normally be zero; any increment triggers an alarm requiring engineering review.

### Counter Definitions

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
| `ctrCgiOrdered` | CGI report orders issued | 0–2³¹ | int64 |
| `ctrCgiReportOk` | CGI reports successfully received | 0–2³¹ | int64 |
| `ctrCgiReportFail` | CGI orders failed (timer expiry or UE failure) | 0–2³¹ | int64 |
| `ctrRelationCreated` | Neighbor relations created by ANR | 0–2³¹ | int64 |
| `ctrRelationRemoved` | Neighbor relations removed by ANR | 0–2³¹ | int64 |
| `ctrXnSetupAttempt` | Xn setups triggered by ANR | 0–2³¹ | int64 |
| `ctrXnSetupOk` | Xn setups completed successfully | 0–2³¹ | int64 |
| `ctrRelationHoFailFlagged` | Relations flagged for chronic handover failure | 0–2³¹ | int64 |

# Cross-References

* [Parameters](parameters.md) — For details on parameters such as `cgiReportTimer` and `relationRemovalTime`.
* [Feature Operation](feature-operation.md) — For details on the ANR operational phases, including CGI resolution and relation removal.
