---
type: procedure
resource: data/vodafone-mvp/raw/Energy-Optimized Power Allocation.pdf#deactivation-procedure
title: Deactivation Procedure
description: Describes the step-by-step process, CLI commands, traffic impact, and
  verification steps for deactivating Energy-Optimized Power Allocation.
tags:
- deactivation
- rancli
- energy-optimized-power-allocation
- power-allocation
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-25T14:49:02+00:00'
  source_sha256: b60f15bc0fbcfee8
sources:
- resource: data/vodafone-mvp/raw/Energy-Optimized Power Allocation.pdf
  title: Energy-Optimized Power Allocation
---

This section outlines the steps required to deactivate the Energy-Optimized Power Allocation feature, including CLI commands, system behavior during deactivation, and verification.

## Impact and Behavior

- **Traffic impact:** None.
- **Effect timing:** Deactivation takes effect at the next scheduling occasion. All subsequent allocations transmit at full configured power spectral density.
- **Prerequisites / State:** There is no state to unwind and no cell lock required. Connected UEs simply see their SINR margin return.
- **Licensing:** The license key can remain installed for re-activation.

## Procedure Steps

### 1. Disable Per Cell
Disable the function on the specific cell:
```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A energyPowerAllocMode=DISABLED
```

### 2. Deactivate the Feature
Deactivate the feature control:
```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=EnergyOptPowerAlloc featureState=DEACTIVATED
```

### 3. Verify
Verify the configuration setting and ensure the reduced-allocation counter stops incrementing in the following Result Output Period (ROP):
```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A energyPowerAllocMode
```

# Cross-References

- [Activation Procedure](activation-procedure.md)
- [Parameters](parameters.md)
- [Performance Management](performance-management.md)
