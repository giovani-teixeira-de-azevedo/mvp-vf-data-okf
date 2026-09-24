---
type: concept
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#feature-operation
title: FEATURE OPERATION
description: Describes the operational mechanisms, sequence, and blockage-recovery
  handling of closed-loop power control for FR2 serving cells.
tags:
- power-control
- FR2
- closed-loop
- gNodeB
- scheduler
- TPC
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-24T14:32:31+00:00'
  source_sha256: 4e23d4a6d69a3f81
sources:
- resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
  title: Closed-Loop Power Control High-Band
---

This section details the operational workflow and blockage-recovery mechanisms for high-band closed-loop power control in the gNodeB scheduler.

## Operational Workflow

For each RRC-connected UE with an active FR2 serving cell, the scheduler maintains a per-carrier closed-loop state $f(i)$ as defined in TS 38.213 clause 7.1.

At every scheduling occasion:
1. The uplink SINR estimator produces a filtered per-UE SINR based on PUSCH DMRS and periodic SRS.
2. The controller compares the measured value against `pcTargetSinr` with a hysteresis window of `pcHysteresis` dB.
3. If the measured value falls outside the hysteresis window, a TPC command of ±1 dB (or +3 dB for fast ramp-up after blockage recovery) is embedded in the next uplink DCI.
4. TPC commands accumulate at the UE, adjusting its operating point and transmit power.

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

A blockage-recovery mechanism detects a sudden SINR drop of more than `blockageDetectThr` dB and temporarily authorizes +3 dB steps until the target window is re-entered. This shortens recovery from hand or body blockage from seconds to a few hundred milliseconds.

# Cross-References

- [FEATURE OVERVIEW](feature-overview.md)
- [PARAMETERS](parameters.md)
