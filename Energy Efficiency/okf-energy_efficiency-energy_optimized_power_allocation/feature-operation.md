---
type: concept
resource: data/vodafone-mvp/raw/Energy-Optimized Power Allocation.pdf#feature-operation
title: Feature Operation
description: Details the dynamic power reduction calculation, implicit signaling mechanism,
  and guard protection for Energy-Optimized Power Allocation.
tags:
- power-allocation
- energy-optimization
- pdsch-epre
- headroom
- link-adaptation
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-25T14:48:25+00:00'
  source_sha256: 31e43c9f2b9756a4
sources:
- title: Energy-Optimized Power Allocation
  resource: data/vodafone-mvp/raw/Energy-Optimized Power Allocation.pdf
---

This section details the operational workflow, power reduction formula, implicit signaling mechanism, and guard functionality for Energy-Optimized Power Allocation.

## Headroom Calculation and Power Reduction

At every scheduling occasion, the scheduler passes the candidate UE, its selected Modulation and Coding Scheme (MCS), and its filtered SINR estimate to the power allocation function.

The function calculates available power headroom as follows:

$$\text{headroom} = \text{estimated SINR} - \text{required SINR(MCS)} - \text{powerMargin}$$

If the calculated headroom is positive:
* PDSCH EPRE for the allocation is reduced by $\min(\lfloor\text{headroom}\rfloor, \text{maxPowerReduction})$ dB.
* The reduction is quantized to **1 dB** steps.

## Implicit Signaling and Outer-Loop Adaptation

The applied power offset is signaled implicitly. Because 3GPP NR UEs derive PDSCH demodulation from Demodulation Reference Signals (DMRS) embedded in the same allocation (per 3GPP TS 38.214), no explicit signaling is required, and the UE demodulates correctly at the reduced power level.

The outer-loop link adaptation monitors HARQ feedback at the reduced power level and adjusts the SINR estimate accordingly, closing the feedback loop.

## Sequence Diagram

```mermaid
sequenceDiagram 
    participant UE as UE 
    participant SCH as Scheduler 
    participant PWR as Power Allocation 
    participant RU as Radio Unit 
    UE->>SCH: CQI 14 (large headroom) 
    SCH->>PWR: Allocation: MCS 22, SINR est 26 dB 
    PWR->>PWR: Required 18 dB + margin 3 dB → headroom 5 dB 
    PWR->>RU: PDSCH EPRE −5 dB for this allocation 
    RU-->>UE: PDSCH + DMRS at reduced power 
    UE->>SCH: HARQ ACK (link still closed) 
    Note over PWR,RU: NACK burst → step reduction back by 2 dB
```

## Aggregate Reporting and Guard Mechanism

* **PA Bias Optimization:** The per-cell aggregate power reduction is reported to the radio unit, which utilizes this data for Power Amplifier (PA) bias optimization where supported.
* **Retransmission Guard:** A guard mechanism suspends all power reductions for a UE for a duration defined by `guardTime` after two consecutive NACKs, ensuring retransmission robustness.

# Cross-References

* [Parameters](parameters.md) - Documents configuration parameters including `powerMargin`, `maxPowerReduction`, and `guardTime`.
