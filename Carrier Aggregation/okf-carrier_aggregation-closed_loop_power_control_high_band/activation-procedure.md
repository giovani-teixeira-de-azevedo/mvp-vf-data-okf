---
type: procedure
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#activation-procedure
title: Activation Procedure
description: Outlines the activation procedure, preconditions, rollout recommendations,
  step-by-step instructions, and CLI commands for Closed-Loop Power Control High-Band.
tags:
- activation
- procedure
- rancli
- closed-loop-pc
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-25T16:57:03+00:00'
  source_sha256: d590f884016b6245
sources:
- title: Closed-Loop Power Control High-Band
  resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
---

This section describes the activation procedure for the Closed-Loop Power Control High-Band feature, including traffic impact, preconditions, recommended rollout order, step-by-step instructions, and CLI commands.

## Overview and Preconditions

- **Traffic impact:** None. Activation only arms the control loop; each UE starts from its current open-loop operating point and is moved gradually by ±1 dB steps, so no service interruption or cell lock is required. The procedure can be executed during business hours.
- **Preconditions:** License key FAK-33011 installed, Physical Layer High-Band and Scheduler High-Band active on the node, and a pre-activation PM baseline collected.
- **Recommended rollout order:** Enable on a small FR2 cluster with default parameters, verify TPC Balance and In-Window Ratio after 48 hours, tune `pcTargetSinr` if needed, then expand cluster by cluster.

## Procedure Steps

- **Step 1:** Verifies the license before configuration.
- **Step 2:** Activates the feature control on the node.
- **Step 3:** Enables the loop per sector carrier and sets the target explicitly so the applied configuration is auditable.
- **Step 4:** Verifies.

After activation, confirm that `ctrTpcUpCmds` and `ctrTpcDownCmds` begin incrementing during the next busy period.

## CLI Commands

```bash
# 1. Verify the license key is installed 
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=ClosedLoopPcHighBand licenseState 
# Expected: licenseState=ENABLED (key FAK-33011) 

# 2. Activate the feature 
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=ClosedLoopPcHighBand featureState=ACTIVATED 

# 3. Enable and tune per sector carrier 
rancli set NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled=true 
rancli set NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 pcTargetSinr=12 pcHysteresis=1 

# 4. Verify 
rancli get NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled
```

# Cross-References

- [Deactivation Procedure](deactivation-procedure.md)
- [Feature Dependencies](feature-depedencies.md)
- [Parameters](parameters.md)
- [Performance Management](performance-management.md)
