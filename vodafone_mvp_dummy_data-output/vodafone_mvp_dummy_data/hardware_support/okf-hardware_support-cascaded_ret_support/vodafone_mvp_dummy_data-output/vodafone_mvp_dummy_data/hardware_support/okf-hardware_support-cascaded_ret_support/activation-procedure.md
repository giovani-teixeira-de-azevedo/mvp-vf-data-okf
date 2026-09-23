---
type: procedure
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Step-by-step activation procedure, CLI commands, prerequisites, and verification
  steps for Cascaded RET Support.
tags:
- cascaded-ret
- activation
- procedure
- rancli
- aisg
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T12:42:47+00:00'
  source_sha256: 0d38afc99fc3972b
sources:
- resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
  title: Cascaded RET Support
---

The Activation Procedure details the steps required to enable the Cascaded RET Support feature, trigger an AISG bus scan, calibrate secondary actuators, and verify successful activation.

## Traffic Impact

None. The feature operates entirely on the antenna control bus; user-plane traffic and cell availability are unaffected. Tilt changes performed after activation alter coverage by design, but activation and discovery themselves move no motors.

## Preconditions & Recommended Rollout

### Preconditions
- License key `FAK-51020` installed.
- The baseline RET Support feature activated.
- Site installation records confirming which ports carry cascades.

### Recommended Rollout
1. Activate node-wide.
2. Run a manual scan on one known multi-RET site.
3. Verify all expected `RetDevice` instances appear with correct serial numbers.
4. Enable automatic scanning fleet-wide.

## Step-by-Step Procedure

### Step 1: Verify the license key is installed

```bash
rancli get NodeRoot=1,EquipmentFunction=1,FeatureCtrl=CascadedRetSupport licenseState
```
*Expected result:* `licenseState=ENABLED` (key `FAK-51020`)

### Step 2: Activate the feature

```bash
rancli set NodeRoot=1,EquipmentFunction=1,FeatureCtrl=CascadedRetSupport featureState=ACTIVATED
```

### Step 3: Scan the AISG bus on the target radio unit port

```bash
rancli action NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1 scanAisgBus
```

### Step 4: Calibrate a discovered secondary actuator

Calibrating a newly discovered actuator is mandatory before executing any tilt command.

```bash
rancli action NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1,RetDevice=2 calibrate
```

### Step 5: Verify discovery and calibration state

```bash
rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1 aldCount
rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1,RetDevice=2 calibrationStatus
```

After activation, confirm `ctrAldSupervisedTime` increments for every expected device.

# Cross-References

- [Feature Dependencies](feature-depedencies.md)
- [Performance Management](performance-management.md)
- [Deactivation Procedure](deactivation-procedure.md)
