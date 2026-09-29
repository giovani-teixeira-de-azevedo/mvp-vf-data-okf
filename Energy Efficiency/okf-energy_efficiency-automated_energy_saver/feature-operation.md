---
type: concept
resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf#feature-operation
title: FEATURE OPERATION
description: Details the operational mechanism, forecasting intervals, savings level
  policies, subordinate feature request sequencing, and pre-emptive wake-up control
  of the Automated Energy Saver feature.
tags:
- energy-saver
- load-forecast
- savingsLevel
- wakeupLeadTime
- feature-operation
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-29T16:08:09+00:00'
  source_sha256: 3fae586006521814
sources:
- title: Automated Energy Saver
  resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf
---

This section describes the operational logic and sequence of the Automated Energy Saver (AES) decision engine, including load forecasting, energy state mapping policies, subordinate feature sequencing, and pre-emptive wake-up mechanisms.

## Operational Logic

### Decision Engine Execution and Load Forecasting
The decision engine runs every 15 minutes, aligned to the Result Output Period (ROP) boundary. For each cell, it generates a load forecast for the upcoming interval, expressed in terms of:
- Expected Physical Resource Block (PRB) utilization
- Expected RRC-connected users

### Target Energy State Mapping (`savingsLevel`)
The forecast is mapped to a target energy state according to the operator's configured `savingsLevel` policy:
- **CONSERVATIVE**: Requires the forecast plus a 3-sigma margin to stay below the subordinate feature's entry threshold.
- **BALANCED**: Uses a 2-sigma margin.
- **AGGRESSIVE**: Uses a 1-sigma margin.

### Subordinate Feature Request Order
The decision engine issues state requests to subordinate features in a fixed sequence:
- **Sleep Sequence**: Booster carriers first, Massive MIMO branch sleep second, radio deep sleep last.
- **Wake-up Sequence**: Issued in the reverse order (radio deep sleep first, Massive MIMO branches second, booster carriers last), restoring capacity bottom-up before it is needed.

### Pre-emptive Wake-up Control (`wakeupLeadTime`)
Wake-ups are issued pre-emptively using a configurable lead time (`wakeupLeadTime`) ahead of the forecast load rise. This removes the reactive wake-up latency of subordinate features from the user experience during the morning ramp.

## Feature Operation Sequence Diagram

```mermaid
sequenceDiagram 
    participant AES as Energy Saver Engine 
    participant BCS as Booster Carrier Sleep 
    participant MMS as Massive MIMO Sleep 
    participant RDS as Radio Deep Sleep 
    AES->>AES: ROP boundary: forecast next interval 
    AES->>BCS: Request carrier sleep (cell N1B) 
    BCS-->>AES: Carrier sleeping 
    AES->>MMS: Request partial sleep (cell N1A) 
    MMS-->>AES: Sleep level 1 active 
    Note over AES,RDS: Deep night: forecast near zero 
    AES->>RDS: Request radio deep sleep (radio 2) 
    AES->>AES: Forecast rising (05:45) 
    AES->>RDS: Wake radio 2 (pre-emptive, 30 min lead) 
    AES->>MMS: Restore full branches 
    AES->>BCS: Wake carrier
```

# Cross-References

- [PARAMETERS](parameters.md)
- [FEATURE DEPEDENCIES](feature-depedencies.md)
