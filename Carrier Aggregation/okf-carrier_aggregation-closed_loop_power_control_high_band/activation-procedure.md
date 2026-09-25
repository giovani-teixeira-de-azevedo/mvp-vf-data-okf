---
type: procedure
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#activation-procedure
title: Activation Procedure
description: Outlines the traffic impact, preconditions, rollout strategy, and step-by-step
  CLI commands for activating Closed-Loop Power Control High-Band.
tags:
- activation
- closed-loop-pc
- high-band
- rancli
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-25T11:23:46+00:00'
  source_sha256: d590f884016b6245
sources:
- resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
  title: Closed-Loop Power Control High-Band
---

The activation procedure enables Closed-Loop Power Control High-Band on a node with zero traffic impact, describing preconditions, recommended rollout strategy, and step-by-step CLI execution.

## Overview and Preconditions

* **Traffic Impact:** None. Activation only arms the control loop; each UE starts from its current open-loop operating point and is moved gradually by ±1 dB steps, so no service interruption or cell lock is required. The procedure can be executed during business hours.
* **Preconditions:**
  * License key `FAK-33011` installed.
  * Physical Layer High-Band and Scheduler High-Band active on the node.
  * Pre-activation PM baseline collected.

## Rollout Strategy

1. Enable on a small FR2 cluster with default parameters.
2. Verify TPC Balance and In-Window Ratio after 48 hours.
3. Tune `pcTargetSinr` if needed.
4. Expand cluster by cluster.

## Step-by-Step Activation Procedure

Step 1 verifies the license before configuration; step 2 activates the feature control on the node; step 3 enables the loop per sector carrier and sets the target explicitly so the applied configuration is auditable; step 4 verifies.

### Step 1: Verify License State
Verify that the license key is installed prior to configuration.
```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=ClosedLoopPcHighBand licenseState
# Expected: licenseState=ENABLED (key FAK-33011)
```

### Step 2: Activate Feature Control
Activate feature control at the node level.
```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=ClosedLoopPcHighBand featureState=ACTIVATED
```

### Step 3: Enable Loop and Configure Sector Carrier
Enable the loop per sector carrier and set the target explicitly so the applied configuration is auditable.
```bash
rancli set NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled=true
rancli set NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 pcTargetSinr=12 pcHysteresis=1
```

### Step 4: Verify Sector Carrier Configuration
Verify the configuration on the sector carrier.
```bash
rancli get NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled
```

### Post-Activation Verification
After activation, confirm that `ctrTpcUpCmds` and `ctrTpcDownCmds` begin incrementing during the next busy period.

# Cross-References

* [Parameters](parameters.md)
* [Performance Management](performance-management.md)
* [Deactivation Procedure](deactivation-procedure.md)
