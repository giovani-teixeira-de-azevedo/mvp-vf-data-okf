---
type: concept
resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf#feature-operation
title: FEATURE OPERATION
description: Describes CQI filtering, UE classification into race-to-sleep or relaxed
  classes, scheduling behaviors, link adaptation back-off, and hysteresis mechanisms
  for CQI-based UE energy efficiency.
tags:
- cqi
- ue-energy-efficiency
- scheduler
- race-to-sleep
- relaxed-class
- link-adaptation
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-25T17:14:37+00:00'
  source_sha256: 1752ba89e88f61b7
sources:
- resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf
  title: CQI-Based UE Energy Efficiency Enhancement
---

This section details the operational logic of the CQI-Based UE Energy Efficiency Enhancement feature, including CQI filtering, UE classification into race-to-sleep or relaxed classes, dynamic scheduling, link adaptation adjustments, and class transition hysteresis.

## Operational Logic

### CQI Filtering
For each connected UE, the feature maintains a smoothed wideband CQI estimate using an exponential filter over reported CQI values with time constant `cqiFilterTime`.

### UE Classification and Action

- **Race-to-Sleep Class**:
  - **Condition**: Filtered CQI is at or above `highCqiThr`.
  - **Behavior**: The scheduler aggregates the UE's pending downlink data into the smallest number of slots permitted by buffer and PRB availability, prioritizing wideband allocations. When the buffer empties, the scheduler immediately releases the UE toward C-DRX inactivity.

- **Relaxed Class**:
  - **Condition**: Filtered CQI stays at or below `lowCqiThr` for `lowCqiHoldTimer` seconds.
  - **Behavior**: The UE's periodic CSI report interval is reconfigured from the cell default to `relaxedCsiPeriod`. Downlink link adaptation applies a fixed back-off of `mcsBackoff` MCS indices to cut HARQ retransmission probability roughly in half.

### Class Transition Hysteresis
Class transitions incorporate hysteresis defined by `cqiClassHysteresis` to prevent UEs on the class boundary from oscillating between configurations, avoiding unnecessary RRC signaling overhead and UE energy expenditure.

## Interaction Sequence

```mermaid
sequenceDiagram 
    participant UE as UE 
    participant SCH as gNodeB Scheduler 
    participant LA as Link Adaptation 
    UE->>SCH: Periodic CQI reports (filtered CQI = 13) 
    SCH->>SCH: Classify UE as race-to-sleep 
    SCH->>UE: Compact burst: 2 slots, wideband allocation 
    UE->>UE: Buffer empty, C-DRX inactivity timer runs out 
    Note over UE: UE sleeps earlier 
    UE->>SCH: Filtered CQI drops to 4 (sustained) 
    SCH->>UE: RRC reconfiguration: CSI period 80 ms 
    SCH->>LA: Apply mcsBackoff = 2 
    LA-->>SCH: Lower initial BLER target, fewer HARQ rounds
```

# Cross-References

- [Parameters](parameters.md)
