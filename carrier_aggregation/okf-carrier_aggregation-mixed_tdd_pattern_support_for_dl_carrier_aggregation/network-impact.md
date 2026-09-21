---
type: concept
resource: data/vodafone-mvp/raw/Mixed TDD Pattern Support for DL Carrier Aggregation.pdf#network-impact
title: Network Impact
description: Analysis of the network impact of Mixed TDD Pattern Support for DL Carrier
  Aggregation across capacity, latency, PUCCH load, throughput, and KPIs.
tags:
- tdd
- carrier-aggregation
- network-impact
- throughput
- latency
- kpi
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:38:52+00:00'
  source_sha256: 81c388d7684ae049
sources:
- title: Mixed TDD Pattern Support for DL Carrier Aggregation
  resource: data/vodafone-mvp/raw/Mixed TDD Pattern Support for DL Carrier Aggregation.pdf
---

This section outlines the network-level impact of enabling Mixed TDD Pattern Support for Downlink Carrier Aggregation (DL CA). Introducing different TDD patterns on the Secondary Cell (SCell) affects key network areas including capacity, latency, physical uplink control channel (PUCCH) load, throughput, and key performance indicators (KPIs).

### Network Impact Details

*   **Capacity**: The carrier with a different TDD pattern becomes usable for Carrier Aggregation (CA). As a result, the downlink CA capacity of the site typically increases by the full capacity of that newly aggregated carrier.
*   **Latency**: The Hybrid Automatic Repeat Request Round-Trip Time (HARQ RTT) on mixed SCell slots increases by 1 to 3 slots. While the impact on user-perceived latency is negligible for Mobile Broadband (MBB) traffic, latency-critical slices should be pinned to the Primary Cell (PCell) using *NR QoS-Aware Downlink Carrier Aggregation*.
*   **PUCCH Load**: Utilization of the PCell PUCCH rises because it must carry feedback for a larger number of downlink slots. It is recommended to monitor PUCCH utilization and enable dual PUCCH groups if necessary.
*   **Throughput**: Due to increased feedback latency, the effective throughput of the mixed SCell is 3% to 7% lower than same-pattern equivalents. However, the aggregate User Equipment (UE) throughput still increases substantially.
*   **KPIs**: Average HARQ RTT per node is expected to rise. This change is structural due to the mixed slot patterns and does not represent a network performance degradation.

# Cross-References

*   [Feature Overview](feature-overview.md)
*   [Performance Management](performance-management.md)
