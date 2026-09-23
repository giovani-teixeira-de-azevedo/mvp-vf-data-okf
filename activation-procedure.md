---
type: procedure
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Provides step-by-step activation commands, preconditions, rollout recommendations,
  and verification procedures for Cascaded RET Support.
tags:
- activation
- procedure
- cascaded-ret
- aisg
- rancli
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T18:53:08+00:00'
  source_sha256: 0d38afc99fc3972b
sources:
- title: Cascaded RET Support
  resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
---

This section describes the activation procedure, preconditions, recommended rollout strategy, and verification steps for enabling the Cascaded RET Support feature.

## Traffic Impact

Traffic impact: none. The feature operates entirely on the antenna control bus; user-plane traffic and cell availability are unaffected. Tilt changes performed after activation alter coverage by design, but activation and discovery themselves move no motors.

## Preconditions

- License key `FAK-51020` installed
- The baseline RET Support feature activated
- Site installation records confirming which ports carry cascades

## Recommended Rollout

Activate node-wide, run a manual scan on one known multi-RET site, verify all expected `RetDevice` instances appear with correct serial numbers, then enable automatic scanning fleet-wide.

## Activation Steps

1. **Verify the license key is installed**
   ```bash
   rancli get NodeRoot=1,EquipmentFunction=1,FeatureCtrl=CascadedRetSupport licenseState
   ```
   *Expected:* `licenseState=ENABLED` (key `FAK-51020`)

2. **Activate the feature**
   ```bash
   rancli set NodeRoot=1,EquipmentFunction=1,FeatureCtrl=CascadedRetSupport featureState=ACTIVATED
   ```

3. **Scan the AISG bus on the target radio unit port**
   ```bash
   rancli action NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1 scanAisgBus
   ```

4. **Calibrate a discovered secondary actuator** (mandatory before any tilt command)
   ```bash
   rancli action NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1,RetDevice=2 calibrate
   ```

5. **Verify discovery and calibration state**
   ```bash
   rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1 aldCount
   rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1,RetDevice=2 calibrationStatus
   ```

## Post-Activation Verification

After activation, confirm `ctrAldSupervisedTime` increments for every expected device.

# Cross-References

- [FEATURE DEPEDENCIES](feature-depedencies.md)
- [PERFORMANCE MANAGEMENT](performance-management.md)
- [DEACTIVATION PROCEDURE](deactivation-procedure.md)
