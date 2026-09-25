---
type: procedure
resource: data/vodafone-mvp/raw/NR Massive MIMO Sleep Mode.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Step-by-step activation procedure, preconditions, and verification commands
  for NR Massive MIMO Sleep Mode.
tags:
- activation
- rancli
- nr-massive-mimo-sleep-mode
- licensing
- configuration
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-25T16:21:03+00:00'
  source_sha256: cbc496fb0074203c
sources:
- resource: data/vodafone-mvp/raw/NR Massive MIMO Sleep Mode.pdf
  title: NR Massive MIMO Sleep Mode
---

The Activation Procedure details the prerequisites, recommended rollout strategy, step-by-step CLI commands, and post-activation verification steps for enabling NR Massive MIMO Sleep Mode.

## Traffic Impact and Execution

* **Traffic Impact:** None. Activating the feature only arms the sleep logic; no branch is gated at activation time, and sleep entry itself occurs only when the cell is already at low load.
* **Execution Window:** The procedure can be executed during business hours.

## Preconditions

* The license key **FAK-31240** must be installed via the normal license management flow.
* The target cells must be running a Massive MIMO baseline feature.
* **NR Flexible Cell Shaping High-Band** must not be enabled on the same cells.

## Recommended Rollout Strategy

1. Activate on a small cluster with `sleepMode=PARTIAL_ONLY`.
2. Observe the KPIs for one to two weeks.
3. Widen configuration to `PARTIAL_AND_DEEP` and to further cells.

## Step-by-Step Activation Procedure

### Step 1: Verify License Key Installation
Confirms the license is enabled before any configuration is touched. Activating a feature with a missing key leaves it in a `licenseState=DISABLED` limbo that is easy to overlook.

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=MassiveMimoSleep licenseState
```
* **Expected Result:** `licenseState=ENABLED (key FAK-31240)`

### Step 2: Activate the Feature Node-Wide
Arms the feature node-wide.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=MassiveMimoSleep featureState=ACTIVATED
```

### Step 3: Enable and Tune Per Cell
Enables and tunes the feature per cell. Thresholds shown below are the default values, listed explicitly so the intended configuration is auditable.

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A sleepMode=PARTIAL_AND_DEEP
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A prbLoadEnterThr=10 prbLoadExitThr=25
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A sleepEnterTimer=300
```

### Step 4: Verify Configuration
Verifies the result.

```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A sleepMode
```

### Post-Activation Verification
After activation, confirm within the next low-traffic period that `ctrSleepEntries` starts incrementing.

# Cross-References

* [Feature Dependencies](feature-depedencies.md)
* [Parameters](parameters.md)
* [Performance Management](performance-management.md)
* [Deactivation Procedure](deactivation-procedure.md)
