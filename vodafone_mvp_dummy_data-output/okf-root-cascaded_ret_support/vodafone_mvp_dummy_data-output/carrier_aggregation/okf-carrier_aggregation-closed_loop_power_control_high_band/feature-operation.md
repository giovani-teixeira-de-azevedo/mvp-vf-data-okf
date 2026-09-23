---
type: concept
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#feature-operation
title: FEATURE OPERATION
description: Describes the operation of Closed-Loop Power Control High-Band including
  SINR estimation, TPC command calculation, sequence flows, and blockage recovery.
tags:
- closed-loop-power-control
- FR2
- TPC
- SINR
- blockage-recovery
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T14:54:56+00:00'
  source_sha256: 4e23d4a6d69a3f81
sources:
- title: Closed-Loop Power Control High-Band
  resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
---

For each RRC-connected UE with an active FR2 serving cell, the scheduler maintains a per-carrier closed-loop state f(i) as defined in TS 38.213 clause 7.1. Every scheduling occasion, the uplink SINR estimator produces a filtered per-UE SINR based on PUSCH DMRS and periodic SRS. The controller compares this against `pcTargetSinr` with a hysteresis window of `pcHysteresis` dB. If the measured value is outside the window, a TPC command of ±1 dB (or +3 dB for fast ramp-up after blockage recovery) is embedded in the next uplink DCI. Commands accumulate at the UE, moving its operating point.

```mermaid
sequenceDiagram 
    participant UE 
    participant PHY as gNodeB PHY 
    participant SCH as gNodeB Scheduler 
    UE->>PHY: PUSCH + DMRS / SRS 
    PHY->>SCH: Filtered per-UE SINR, RSSI 
    SCH->>SCH: Compare vs pcTargetSinr ± pcHysteresis 
    SCH->>UE: DCI 0_1 with TPC command 
    UE->>UE: Accumulate f(i), adjust Tx power 
    Note over UE,SCH: Loop reset on beam switch indication
```

A blockage-recovery mechanism detects a sudden SINR drop of more than `blockageDetectThr` dB and temporarily authorizes +3 dB steps until the target window is re-entered, shortening recovery from hand or body blockage from seconds to a few hundred milliseconds.

# Cross-References

- [FEATURE OVERVIEW](feature-overview.md)
- [PARAMETERS](parameters.md)
