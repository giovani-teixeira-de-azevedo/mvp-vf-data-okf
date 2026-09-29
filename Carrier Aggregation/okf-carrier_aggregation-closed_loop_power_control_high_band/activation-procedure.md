---
type: procedure
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#activation-procedure
title: Activation Procedure
description: Step-by-step activation procedure, preconditions, traffic impact, rollout
  guidance, and CLI commands for Closed-Loop Power Control High-Band.
tags:
- closed-loop-pc
- high-band
- fr2
- activation
- rancli
- configuration
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-29T13:11:47+00:00'
  source_sha256: d590f884016b6245
sources:
- resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
  title: Closed-Loop Power Control High-Band
---

This section outlines the activation procedure for Closed-Loop Power Control High-Band, including traffic impact, preconditions, rollout recommendations, CLI execution commands, and post-activation verification steps.

## Traffic Impact

* **Impact:** None.
* **Service Interruption:** No service interruption or cell lock is required.
* **Execution Window:** Can be executed during business hours.
* **Behavior:** Activation only arms the control loop; each UE starts from its current open-loop operating point and is moved gradually by ±1 dB steps.

## Preconditions

* License key **FAK-33011** installed.
* **Physical Layer High-Band** and **Scheduler High-Band** active on the node.
* Pre-activation PM baseline collected.

## Recommended Rollout Order

1. Enable on a small FR2 cluster with default parameters.
2. Verify TPC Balance and In-Window Ratio after 48 hours.
3. Tune `pcTargetSinr` if needed.
4. Expand cluster by cluster.

## Procedure Steps

### Step 1: Verify the License Key is Installed
Verifies the license before configuration:

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=ClosedLoopPcHighBand licenseState
```

**Expected:** `licenseState=ENABLED` (key FAK-33011)

### Step 2: Activate the Feature
Activates the feature control on the node:

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=ClosedLoopPcHighBand featureState=ACTIVATED
```

### Step 3: Enable and Tune Per Sector Carrier
Enables the loop per sector carrier and sets the target explicitly so the applied configuration is auditable:

```bash
rancli set NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled=true
rancli set NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 pcTargetSinr=12 pcHysteresis=1
```

### Step 4: Verify
Verifies the configuration on the sector carrier:

```bash
rancli get NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled
```

## Post-Activation Verification

After activation, confirm that performance counters `ctrTpcUpCmds` and `ctrTpcDownCmds` begin incrementing during the next busy period.

# Cross-References

* [Feature Overview](feature-overview.md)
* [Parameters](parameters.md)
* [Performance Management](performance-management.md)
* [Deactivation Procedure](deactivation-procedure.md)
