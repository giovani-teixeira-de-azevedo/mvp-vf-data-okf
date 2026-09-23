---
type: procedure
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Outlines the steps, preconditions, commands, and verification checks
  required to activate Closed-Loop Power Control High-Band on a node and sector carrier.
tags:
- activation
- closed-loop-pc
- rancli
- fr2
- power-control
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T12:54:00+00:00'
  source_sha256: d590f884016b6245
sources:
- title: Closed-Loop Power Control High-Band
  resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
---

This section details the activation procedure for the Closed-Loop Power Control High-Band feature, including traffic impact considerations, preconditions, rollout recommendations, step-by-step CLI execution commands, and post-activation verification.

## Traffic Impact & Preconditions

### Traffic Impact
* **Impact:** None.
* Activation only arms the control loop; each UE starts from its current open-loop operating point and is moved gradually by ±1 dB steps.
* No service interruption or cell lock is required.
* The procedure can be executed during business hours.

### Preconditions
* License key `FAK-33011` installed.
* Physical Layer High-Band and Scheduler High-Band active on the node.
* Pre-activation PM baseline collected.

### Recommended Rollout Order
1. Enable on a small FR2 cluster with default parameters.
2. Verify TPC Balance and In-Window Ratio after 48 hours.
3. Tune `pcTargetSinr` if needed.
4. Expand cluster by cluster.

## Step-by-Step Activation Procedure

### Step 1: Verify the license key is installed
Verifies the license before configuration.
```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=ClosedLoopPcHighBand licenseState
# Expected: licenseState=ENABLED (key FAK-33011)
```

### Step 2: Activate the feature
Activates the feature control on the node.
```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=ClosedLoopPcHighBand featureState=ACTIVATED
```

### Step 3: Enable and tune per sector carrier
Enables the loop per sector carrier and sets the target explicitly so the applied configuration is auditable.
```bash
rancli set NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled=true
rancli set NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 pcTargetSinr=12 pcHysteresis=1
```

### Step 4: Verify configuration
Verifies the sector carrier configuration.
```bash
rancli get NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled
```

## Post-Activation Verification

After activation, confirm that counters `ctrTpcUpCmds` and `ctrTpcDownCmds` begin incrementing during the next busy period.

# Cross-References

* [Deactivation Procedure](deactivation-procedure.md)
* [Parameters](parameters.md)
* [Performance Management](performance-management.md)
