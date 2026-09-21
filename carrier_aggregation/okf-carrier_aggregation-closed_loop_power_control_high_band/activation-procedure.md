---
type: procedure
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Step-by-step activation and verification procedure for the Closed-Loop
  Power Control High-Band feature.
tags:
- activation
- rancli
- power-control
- high-band
- procedure
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:37:02+00:00'
  source_sha256: d590f884016b6245
sources:
- resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
  title: Closed-Loop Power Control High-Band
---

This section describes the step-by-step activation procedure for the Closed-Loop Power Control High-Band feature, including traffic impact considerations, preconditions, recommended rollout strategy, and command line execution steps.

## Overview and Preconditions

- **Traffic Impact:** None. Activation only arms the control loop. Each UE starts from its current open-loop operating point and is moved gradually by ±1 dB steps, so no service interruption or cell lock is required. The procedure can be executed during business hours.
- **Preconditions:**
  - License key `FAK-33011` installed.
  - Physical Layer High-Band and Scheduler High-Band active on the node.
  - Pre-activation PM baseline collected.
- **Recommended Rollout Order:**
  1. Enable on a small FR2 cluster with default parameters.
  2. Verify TPC Balance and In-Window Ratio after 48 hours.
  3. Tune `pcTargetSinr` if needed.
  4. Expand cluster by cluster.

---

## Execution Steps

The procedure consists of verifying the license, activating the feature control, enabling the loop per sector carrier, and verifying the applied configuration.

### Step 1: Verify the license key is installed 
```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=ClosedLoopPcHighBand licenseState 
```
**Expected Output:**
`licenseState=ENABLED` (key FAK-33011)

### Step 2: Activate the feature 
```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=ClosedLoopPcHighBand featureState=ACTIVATED 
```

### Step 3: Enable and tune per sector carrier 
This step enables the loop per sector carrier and sets the target explicitly so that the applied configuration is auditable.
```bash
rancli set NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled=true 
rancli set NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 pcTargetSinr=12 pcHysteresis=1 
```

### Step 4: Verify 
```bash
rancli get NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled
```

### Post-Activation Verification
After activation, confirm that the performance counters `ctrTpcUpCmds` and `ctrTpcDownCmds` begin incrementing during the next busy period.

# Cross-References

- [Feature Dependencies](feature-depedencies.md) — Precondition requirements.
- [Parameters](parameters.md) — Details on parameters adjusted during step 3 (`closedLoopPcEnabled`, `pcTargetSinr`, `pcHysteresis`).
- [Performance Management](performance-management.md) — Information on counters `ctrTpcUpCmds`, `ctrTpcDownCmds` and metrics like TPC Balance and In-Window Ratio.
- [Deactivation Procedure](deactivation-procedure.md) — Instructions to roll back or deactivate the feature.
