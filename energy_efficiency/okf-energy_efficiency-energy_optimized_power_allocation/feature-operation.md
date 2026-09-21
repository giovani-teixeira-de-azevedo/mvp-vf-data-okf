---
type: concept
resource: data/vodafone-mvp/raw/Energy-Optimized Power Allocation.pdf#feature-operation
title: FEATURE OPERATION
description: Detailed mechanism of the energy-optimized power allocation function
  including headroom computation, implicit signaling, and the guard mechanism.
tags:
- power-allocation
- energy-optimization
- sinr
- harq
- epre
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:39:22+00:00'
  source_sha256: 31e43c9f2b9756a4
sources:
- resource: data/vodafone-mvp/raw/Energy-Optimized Power Allocation.pdf
  title: Energy-Optimized Power Allocation
---

The **Feature Operation** section details the algorithmic execution of the Energy-Optimized Power Allocation feature. It outlines the step-by-step calculation performed during each scheduling occasion, the role of implicit signaling, and the built-in guard mechanism designed to maintain link robustness.

## Power Allocation Algorithm

For every scheduling occasion, the scheduler provides the candidate UE, its selected Modulation and Coding Scheme (MCS), and its filtered SINR estimate to the power allocation function. 

The function computes the available headroom as follows:

$$\text{headroom} = \text{estimated SINR} - \text{required SINR(MCS)} - \text{powerMargin}$$

* **If headroom is positive:** The Physical Downlink Shared Channel (PDSCH) Energy Per Resource Element (EPRE) for the allocation is reduced by:
  $$\text{Reduction (dB)} = \min(\lfloor\text{headroom}\rfloor, \text{maxPowerReduction})$$
  This reduction is quantized to 1 dB steps.
* **If headroom is negative or zero:** No power reduction is applied.

### Implicit Signaling

The applied power offset is signaled implicitly to the UE. Because New Radio (NR) UEs derive PDSCH demodulation from Demodulation Reference Signals (DMRS) embedded in the same physical allocation (per 3GPP TS 38.214), no additional downlink control signaling is required. The UE demodulates the transmission correctly at the reduced power.

Subsequently, the outer-loop link adaptation (OLLA) observes Hybrid Automatic Repeat Request (HARQ) feedback at this reduced power and adjusts the SINR estimate accordingly, closing the loop.

---

## Signal Flow Diagram

The following sequence diagram illustrates the message flow and calculation steps between the UE, Scheduler, Power Allocation function, and Radio Unit (RU):

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

---

## Radio Integration and Guard Mechanisms

### Power Amplifier (PA) Bias Optimization
The per-cell aggregate power reduction is calculated and reported to the radio hardware. Where supported by the vendor, the radio utilizes this aggregate report to optimize Power Amplifier (PA) bias dynamically, achieving hardware-level energy savings.

### Retransmission Guard Mechanism
To ensure link robustness and avoid consecutive packet drops, a guard mechanism is implemented:
* If a UE experiences **two consecutive NACKs**, all power reductions for that UE are suspended.
* The suspension remains in effect for the duration of the configured `guardTime` parameter.

# Cross-References

* [FEATURE OVERVIEW](feature-overview.md) — For the high-level context and objectives of the energy-optimized power allocation feature.
* [PARAMETERS](parameters.md) — For definitions of the control parameters used in the headroom calculation and guard mechanism (`powerMargin`, `maxPowerReduction`, and `guardTime`).
* [PERFORMANCE MANAGEMENT](performance-management.md) — For information on how these power adjustments and NACK events impact cell-level KPIs.
