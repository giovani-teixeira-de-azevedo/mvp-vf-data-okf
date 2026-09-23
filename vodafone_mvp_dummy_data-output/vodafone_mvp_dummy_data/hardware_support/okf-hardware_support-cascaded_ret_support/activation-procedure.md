---
type: procedure
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Activation procedure, CLI execution commands, calibration steps, and
  verification for Cascaded RET Support.
tags:
- activation
- cascaded-ret
- aisg
- rancli
- calibration
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T15:10:33+00:00'
  source_sha256: 0d38afc99fc3972b
sources:
- resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
  title: Cascaded RET Support
---

This document details the activation procedure, preconditions, traffic impact, recommended rollout, and verification steps for Cascaded RET Support.

## Traffic Impact

Traffic impact is none. The feature operates entirely on the antenna control bus; user-plane traffic and cell availability are unaffected. Tilt changes performed after activation alter coverage by design, but activation and discovery themselves move no motors.

## Preconditions

- License key FAK-51020 installed
- The baseline RET Support feature activated
- Site installation records confirming which ports carry cascades

## Recommended Rollout

Activate node-wide, run a manual scan on one known multi-RET site, verify all expected `RetDevice` instances appear with correct serial numbers, then enable automatic scanning fleet-wide.

## Step-by-Step Activation Procedure

### Step 1: Verify the license key is installed

```bash
rancli get NodeRoot=1,EquipmentFunction=1,FeatureCtrl=CascadedRetSupport licenseState
```

Expected output: `licenseState=ENABLED` (key FAK-51020)

### Step 2: Activate the feature

```bash
rancli set NodeRoot=1,EquipmentFunction=1,FeatureCtrl=CascadedRetSupport featureState=ACTIVATED
```

### Step 3: Scan the AISG bus on the target radio unit port

```bash
rancli action NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1 scanAisgBus
```

### Step 4: Calibrate a discovered secondary actuator

Calibration of a newly discovered actuator is mandatory before any tilt command.

```bash
rancli action NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1,RetDevice=2 calibrate
```

### Step 5: Verify discovery and calibration state

```bash
rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1 aldCount
rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1,RetDevice=2 calibrationStatus
```

After activation, confirm `ctrAldSupervisedTime` increments for every expected device.

## Cross-References

- [Feature Dependencies](feature-depedencies.md)
- [Deactivation Procedure](deactivation-procedure.md)
- [Performance Management](performance-management.md)
