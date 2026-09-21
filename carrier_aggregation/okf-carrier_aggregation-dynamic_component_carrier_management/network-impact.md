---
type: concept
resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf#network-impact
title: Network Impact
description: Outlines the capacity, load balancing, signaling, user experience, and
  KPI impacts of activating Dynamic Component Carrier Management.
tags:
- capacity
- throughput
- load-balancing
- rrc-reconfiguration
- scell-swap
- kpi
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:37:57+00:00'
  source_sha256: 8e95be339fe808b8
sources:
- title: Dynamic Component Carrier Management
  resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf
---

This section details the system-level and user-level impacts of activating the Dynamic Component Carrier Management feature, including capacity, signaling, load balancing, user experience, and key performance indicators (KPIs).

## Key Impact Areas

*   **Capacity**: Busy-hour aggregate downlink throughput improves by 10% to 25% on sites with three or more carriers. This enhancement is driven by more efficient utilization of secondary carriers (SCells).
*   **Load Balance**: Physical Resource Block (PRB) utilization spread across carriers narrows substantially. As a result, alarms and congestion events on the historically first-priority carrier decrease.
*   **Signaling**: RRC Reconfiguration volume increases modestly, typically by 5% to 10%, due to rebalancing swaps. The pacing cap parameter keeps this additional signaling bounded (see [Parameters](parameters.md)).
*   **End Users**: Individual User Equipments (UEs) occasionally experience an SCell swap, which interrupts SCell data for approximately 20 to 30 ms. PCell data transmission continues uninterrupted throughout this swap process.
*   **KPIs**: Per-carrier throughput KPIs converge after activation. Consequently, per-carrier trends must be interpreted jointly rather than individually (see [Performance Management](performance-management.md)).

# Cross-References

*   [Feature Operation](feature-operation.md) — For details on how secondary carriers are selected and swapped.
*   [Parameters](parameters.md) — For configuring the pacing cap and other thresholds governing carrier swaps.
*   [Performance Management](performance-management.md) — For observing joint per-carrier trends and performance monitoring.
