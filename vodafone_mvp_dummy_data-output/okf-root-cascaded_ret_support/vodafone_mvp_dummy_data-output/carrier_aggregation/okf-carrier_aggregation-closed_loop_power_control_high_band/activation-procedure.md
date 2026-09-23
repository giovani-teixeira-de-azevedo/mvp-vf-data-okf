---
type: procedure
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Activation procedure, preconditions, traffic impact, rollout strategy,
  and execution steps for Closed-Loop Power Control High-Band.
tags:
- activation
- procedure
- rancli
- power-control
- fr2
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T14:57:16+00:00'
  source_sha256: d590f884016b6245
sources:
- resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
  title: Closed-Loop Power Control High-Band
---

Traffic impact: none. Activation only arms the control loop; each UE starts from its current open-loop operating point and is moved gradually by ±1 dB steps, so no service interruption or cell lock is required. The procedure can be executed during business hours.

Preconditions: license key FAK-33011 installed, Physical Layer High-Band and Scheduler High-Band active on the node, and a pre-activation PM baseline collected. Recommended rollout order: enable on a small FR2 cluster with default parameters, verify TPC Balance and In-Window Ratio after 48 hours, tune `pcTargetSinr` if needed, then expand cluster by cluster.

Step 1 verifies the license before configuration; step 2 activates the feature control on the node; step 3 enables the loop per sector carrier and sets the target explicitly so the applied configuration is auditable; step 4 verifies. After activation, confirm that `ctrTpcUpCmds` and `ctrTpcDownCmds` begin incrementing during the next busy period.

## 1. Verify the license key is installed

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=ClosedLoopPcHighBand licenseState
```
Expected output: `licenseState=ENABLED (key FAK-33011)`

## 2. Activate the feature

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=ClosedLoopPcHighBand featureState=ACTIVATED
```

## 3. Enable and tune per sector carrier

```bash
rancli set NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled=true
rancli set NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 pcTargetSinr=12 pcHysteresis=1
```

## 4. Verify

```bash
rancli get NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled
```
