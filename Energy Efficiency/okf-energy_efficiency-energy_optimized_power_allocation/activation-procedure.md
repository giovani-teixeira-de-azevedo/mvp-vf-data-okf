---
type: procedure
resource: data/vodafone-mvp/raw/Energy-Optimized Power Allocation.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Activation procedure, prerequisites, CLI commands, and post-change verification
  for Energy-Optimized Power Allocation.
tags:
- activation
- procedure
- rancli
- power-allocation
- energy-saving
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-25T14:49:03+00:00'
  source_sha256: be6de7a37ed90adc
sources:
- resource: data/vodafone-mvp/raw/Energy-Optimized Power Allocation.pdf
  title: Energy-Optimized Power Allocation
---

This document details the activation procedure for the Energy-Optimized Power Allocation feature, including traffic impact considerations, preconditions, rollout recommendations, execution commands, and post-change verification.

## Prerequisites and Operational Considerations

- **Traffic Impact:** None. The feature only reduces power on links with verified headroom, one allocation at a time, and the closed-loop guard restores full power on any sign of degradation. No cell lock or restart is required; the procedure can be executed during business hours.
- **Preconditions:**
  - License key `FAK-31530` must be installed.
  - If **EMF Power Lock Mid-Band** or **Modulation-Aware Power Control** is active on the target cells, confirm their configuration first so the combined power behavior is understood.
- **Recommended Rollout Strategy:**
  - Activate on a pilot cluster at default margin.
  - Verify BLER and throughput KPIs at baseline for one week before extending network-wide.
  - Tighten `powerMargin` only per-cluster and one dB at a time.

## Execution Steps

Step 1 verifies the license; step 2 activates the feature control; step 3 enables per cell with explicit margin values for auditability; step 4 verifies.

```bash
# 1. Verify the license key is installed 
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=EnergyOptPowerAlloc licenseState 
# Expected: licenseState=ENABLED (key FAK-31530) 
 
# 2. Activate the feature 
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=EnergyOptPowerAlloc featureState=ACTIVATED 
 
# 3. Enable and tune per cell 
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A energyPowerAllocMode=ENABLED 
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A powerMargin=3 maxPowerReduction=4 
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A paBiasTracking=ON 
 
# 4. Verify 
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A energyPowerAllocMode
```

## Post-Change Verification

Post-change, confirm `ctrReducedAllocs` increments within minutes under traffic, and verify that the ratio `ctrGuardEvents / ctrReducedAllocs` stays below 2%.

# Cross-References

- [Deactivation Procedure](deactivation-procedure.md)
- [Parameters](parameters.md)
- [Performance Management](performance-management.md)
