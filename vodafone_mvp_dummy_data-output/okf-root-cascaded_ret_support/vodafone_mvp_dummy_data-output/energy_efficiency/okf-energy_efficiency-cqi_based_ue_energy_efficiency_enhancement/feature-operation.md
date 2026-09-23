---
type: concept
resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf#feature-operation
title: Feature Operation
description: Details the operational mechanisms, UE classification, scheduler optimizations,
  link adaptation, and sequence interactions for CQI-Based UE Energy Efficiency Enhancement.
tags:
- cqi
- energy-efficiency
- race-to-sleep
- relaxed-class
- scheduler
- link-adaptation
- c-drx
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T14:17:44+00:00'
  source_sha256: 1752ba89e88f61b7
sources:
- title: CQI-Based UE Energy Efﬁciency Enhancement
  resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf
---

This section details the operational logic and UE classification mechanisms used by the CQI-Based UE Energy Efficiency Enhancement feature, including scheduler optimizations and link adaptation adjustments.

## UE Classification and CQI Filtering

Per connected UE, the feature maintains a smoothed wideband CQI estimate computed using an exponential filter over reported CQI values with time constant `cqiFilterTime`. Based on the filtered CQI, UEs are assigned to specific operational classes:

### Race-to-Sleep Class
UEs whose filtered CQI is at or above `highCqiThr` are placed in the race-to-sleep class:
- **Scheduling Optimization**: The gNodeB scheduler aggregates pending downlink data into the smallest number of slots allowed by buffer and PRB availability, prioritizing wideband allocations.
- **C-DRX Transition**: When the buffer empties, the scheduler immediately releases the UE toward C-DRX inactivity, allowing the UE to enter sleep mode earlier.

### Relaxed Class
UEs whose filtered CQI stays at or below `lowCqiThr` for `lowCqiHoldTimer` seconds are placed in the relaxed class:
- **CSI Reporting Frequency**: The periodic CSI report interval is reconfigured from the cell default to `relaxedCsiPeriod`.
- **Link Adaptation Back-off**: Downlink link adaptation applies a fixed back-off of `mcsBackoff` MCS indices to cut HARQ retransmission probability roughly in half.

## Class Transition Hysteresis

Class transitions incorporate hysteresis (`cqiClassHysteresis`) so that UEs on the class boundary do not oscillate between configurations, which would itself cost RRC signaling and UE energy.

## Operation Sequence

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

- [Feature Overview](feature-overview.md)
- [Parameters](parameters.md)
