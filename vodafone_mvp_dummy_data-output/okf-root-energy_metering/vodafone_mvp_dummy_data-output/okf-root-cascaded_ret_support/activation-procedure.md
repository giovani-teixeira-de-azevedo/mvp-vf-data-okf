---
type: procedure
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Activation procedure, preconditions, rollout strategy, and verification
  commands for Cascaded RET Support.
tags:
- cascaded-ret
- activation
- rancli
- aisg
- ret-device
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T10:08:32+00:00'
  source_sha256: 0d38afc99fc3972b
sources:
- title: Cascaded RET Support
  resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
---

This section provides the activation procedure, preconditions, recommended rollout strategy, traffic impact, and step-by-step CLI commands for enabling Cascaded RET Support.

## Traffic Impact and Preconditions

### Traffic Impact
None. The feature operates entirely on the antenna control bus; user-plane traffic and cell availability are unaffected. Tilt changes performed after activation alter coverage by design, but activation and discovery themselves move no motors.

### Preconditions
- License key `FAK-51020` installed
- Baseline RET Support feature activated
- Site installation records confirming which ports carry cascades

### Recommended Rollout
Activate node-wide, run a manual scan on one known multi-RET site, verify all expected `RetDevice` instances appear with correct serial numbers, then enable automatic scanning fleet-wide.

## Step-by-Step Procedure

The activation procedure consists of five steps:
1. Verify the license key is installed.
2. Activate the feature control.
3. Trigger an AISG bus scan on the target port.
4. Calibrate a newly discovered actuator (mandatory before issuing any tilt command).
5. Verify discovery and calibration state.

After activation, confirm that `ctrAldSupervisedTime` increments for every expected device.

### CLI Commands

```bash
# 1. Verify the license key is installed 
rancli get NodeRoot=1,EquipmentFunction=1,FeatureCtrl=CascadedRetSupport licenseState 
# Expected: licenseState=ENABLED (key FAK-51020) 
 
# 2. Activate the feature 
rancli set NodeRoot=1,EquipmentFunction=1,FeatureCtrl=CascadedRetSupport featureState=ACTIVATED 
 
# 3. Scan the AISG bus on the target radio unit port 
rancli action NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1 scanAisgBus 
 
# 4. Calibrate a discovered secondary actuator 
rancli action NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1,RetDevice=2 calibrate 
 
# 5. Verify 
rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1 aldCount 
rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1,RetDevice=2 calibrationStatus
```

# Cross-References

- [FEATURE DEPEDENCIES](feature-depedencies.md)
- [DEACTIVATION PROCEDURE](deactivation-procedure.md)
