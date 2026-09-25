---
type: concept
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#feature-operation
title: FEATURE OPERATION
description: Details the operational mechanism of Closed-Loop Power Control High-Band,
  including per-carrier closed-loop state maintenance, TPC command generation, and
  blockage recovery.
tags:
- power control
- closed-loop
- FR2
- TPC
- SINR
- blockage recovery
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-25T16:49:16+00:00'
  source_sha256: 4e23d4a6d69a3f81
sources:
- title: Closed-Loop Power Control High-Band
  resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
---

This section details the operational process of closed-loop power control and blockage recovery for active FR2 serving cells in high-band deployments.

## Closed-Loop Power Control Process

For each RRC-connected UE with an active FR2 serving cell, the scheduler maintains a per-carrier closed-loop state f(i) as defined in TS 38.213 clause 7.1.

At every scheduling occasion:
1. The uplink SINR estimator produces a filtered per-UE SINR based on PUSCH DMRS and periodic SRS.
2. The controller compares the measured value against `pcTargetSinr` with a hysteresis window of `pcHysteresis` dB.
3. If the measured value is outside the window:
   - A TPC command of ±1 dB (or +3 dB for fast ramp-up after blockage recovery) is embedded in the next uplink DCI.
   - Commands accumulate at the UE, moving its operating point.

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

## Blockage Recovery

A blockage-recovery mechanism detects a sudden SINR drop of more than `blockageDetectThr` dB and temporarily authorizes +3 dB steps until the target window is re-entered, shortening recovery from hand or body blockage from seconds to a few hundred milliseconds.

# Cross-References

- [PARAMETERS](parameters.md)
