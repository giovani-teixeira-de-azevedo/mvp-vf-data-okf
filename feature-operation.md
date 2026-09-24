---
type: concept
resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf#feature-operation
title: FEATURE OPERATION
description: Describes the Automated Energy Saver decision engine operation, load
  forecasting, savings policy levels, feature execution ordering, and pre-emptive
  wake-up mechanisms.
tags:
- automated-energy-saver
- decision-engine
- load-forecasting
- savingslevel
- wakeupleadtime
- booster-carrier-sleep
- massive-mimo-sleep
- radio-deep-sleep
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-24T08:37:08+00:00'
  source_sha256: 3fae586006521814
sources:
- title: Automated Energy Saver
  resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf
---

This section describes the operational logic of the Automated Energy Saver feature, including decision engine scheduling, load forecasting, policy-driven threshold mapping, subordinate feature execution sequence, and pre-emptive wake-up handling.

## Decision Engine and Load Forecasting

The decision engine runs every 15 minutes, aligned to the Result Output Period (ROP) boundary. For each cell, the engine produces a load forecast for the next interval, expressed as:

- Expected Physical Resource Block (PRB) utilization
- Expected RRC-connected users

## Savings Level Policy

The load forecast is mapped to a target energy state according to the operator's `savingsLevel` policy:

| Policy Level | Required Margin | Entry Condition |
| :--- | :--- | :--- |
| **CONSERVATIVE** | 3-sigma margin | Forecast plus 3-sigma margin must stay below the subordinate feature's entry threshold. |
| **BALANCED** | 2-sigma margin | Forecast plus 2-sigma margin must stay below the subordinate feature's entry threshold. |
| **AGGRESSIVE** | 1-sigma margin | Forecast plus 1-sigma margin must stay below the subordinate feature's entry threshold. |

## Subordinate Feature Control Sequence

State requests are issued to subordinate features in a fixed order to manage network energy states safely:

1. **Sleep Sequence:**
   1. Booster carriers first
   2. Massive MIMO branch sleep second
   3. Radio deep sleep last

2. **Wake-Up Sequence:**
   - Reversed relative to sleep order (Radio deep sleep restored first, followed by Massive MIMO branches, and booster carriers last).
   - Restores capacity bottom-up before it is needed.

### Pre-Emptive Wake-Up

Wake-ups are issued pre-emptively with a configurable lead time (`wakeupLeadTime`) ahead of the forecasted load rise. This eliminates the reactive wake-up latency of subordinate features from the user experience during morning traffic ramps.

## Sequence Diagram

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

- [Feature Overview](feature-overview.md)
- [Parameters](parameters.md)
- [Network Impact](network-impact.md)
