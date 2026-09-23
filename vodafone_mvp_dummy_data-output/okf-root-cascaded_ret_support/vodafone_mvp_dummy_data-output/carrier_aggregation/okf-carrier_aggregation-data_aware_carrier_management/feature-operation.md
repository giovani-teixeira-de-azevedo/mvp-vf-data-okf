---
type: concept
resource: data/vodafone-mvp/raw/Data-Aware Carrier Management.pdf#feature-operation
title: Feature Operation
description: Describes the operational mechanism of Data-Aware Carrier Management,
  including demand estimation sampling, threshold-driven class transitions, and SCell
  lifecycle signaling.
tags:
- data-aware-carrier-management
- demand-estimator
- scell-activation
- scell-deactivation
- rrc-reconfiguration
- carrier-aggregation
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T13:48:24+00:00'
  source_sha256: a10c8b442b121dfd
sources:
- resource: data/vodafone-mvp/raw/Data-Aware Carrier Management.pdf
  title: Data-Aware Carrier Management
---

This section describes the operational mechanisms of the Data-Aware Carrier Management feature, including UE demand estimation sampling, threshold-driven class transitions, and associated SCell signaling.

## Demand Estimation and Classification

The demand estimator samples each UE every 20 ms and maintains an exponentially weighted average of:
- Downlink and uplink buffer occupancy
- Size of the last N data bursts
- Inter-burst idle time

Classification thresholds `bulkBurstThr` and `interactiveBurstThr` map these observations to demand classes. Hysteresis is controlled by `classDwellTimer` to prevent oscillation.

## SCell Lifecycle and Signaling

* **Promotion to BULK:** On promotion to `BULK`, the scheduler issues SCell Activation MAC CEs for all configured SCells. If SCells are not yet configured, an RRC Reconfiguration adding them is issued (TS 38.331).
* **Demotion and De-configuration:** On sustained inactivity beyond `demoteTimer`, SCells are deactivated first. After a further duration defined by `deconfigTimer`, SCells are de-configured, returning the UE to a lean PCell-only configuration.

## Sequence Flow

```mermaid
sequenceDiagram 
    participant UE 
    participant SCH as gNodeB Scheduler 
    participant EST as Demand Estimator 
    UE->>SCH: DL data burst arrives in buffer 
    SCH->>EST: Buffer sample (20 ms cadence) 
    EST->>SCH: Class promotion INTERACTIVE -> BULK 
    SCH->>UE: RRC Reconfiguration (add SCells) if needed 
    SCH->>UE: MAC CE SCell Activation 
    UE-->>SCH: CSI on SCells (activation complete ~3 ms FR1) 
    Note over UE,SCH: Burst served with full aggregation 
    EST->>SCH: Idle > demoteTimer 
    SCH->>UE: MAC CE SCell Deactivation
```

# Cross-References

- [Parameters](parameters.md)
- [Activation Procedure](activation-procedure.md)
- [Deactivation Procedure](deactivation-procedure.md)
