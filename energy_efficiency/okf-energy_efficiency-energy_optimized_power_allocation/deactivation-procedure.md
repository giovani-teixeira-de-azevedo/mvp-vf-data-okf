---
type: procedure
resource: data/vodafone-mvp/raw/Energy-Optimized Power Allocation.pdf#deactivation-procedure
title: Deactivation Procedure
description: Detailed step-by-step procedure to disable and deactivate the Energy-Optimized
  Power Allocation feature.
tags:
- deactivation
- power-allocation
- rancli
- energy-saving
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:39:44+00:00'
  source_sha256: b60f15bc0fbcfee8
sources:
- resource: data/vodafone-mvp/raw/Energy-Optimized Power Allocation.pdf
  title: Energy-Optimized Power Allocation
---

This document outlines the step-by-step deactivation procedure for the Energy-Optimized Power Allocation feature. Deactivating the feature has no impact on active user traffic and does not require a cell lock.

## Impact & Key Characteristics

- **Traffic Impact:** None.
- **Timing:** Deactivation takes effect at the next scheduling occasion. All subsequent allocations transmit at the full configured power spectral density.
- **State/Locking:** There is no state to unwind, and no cell lock is required. Connected UEs will simply see their SINR (Signal-to-Interference-plus-Noise Ratio) margin return.
- **Licensing:** The license key can remain installed to allow for future re-activation.

## Step-by-Step Deactivation Procedure

### Step 1: Disable Per Cell
Disable the power allocation mode on the specific cell.

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A energyPowerAllocMode=DISABLED
```

### Step 2: Deactivate the Feature Control
Deactivate the feature state at the function level.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=EnergyOptPowerAlloc featureState=DEACTIVATED
```

### Step 3: Verify Deactivation
Query the cell parameter to verify the state and monitor performance counters in the following Recording Observation Period (ROP) to ensure that the reduced-allocation counter stops incrementing.

```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A energyPowerAllocMode
```

# Cross-References

* [Activation Procedure](activation-procedure.md) - Contains the corresponding activation procedure.
* [Parameters](parameters.md) - Describes parameters like `energyPowerAllocMode` referenced in this procedure.
