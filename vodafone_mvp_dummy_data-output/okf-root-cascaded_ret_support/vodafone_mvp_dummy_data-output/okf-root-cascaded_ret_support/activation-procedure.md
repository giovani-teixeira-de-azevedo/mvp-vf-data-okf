---
type: procedure
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Outlines the procedure for activating Cascaded RET Support, including
  preconditions, rollout strategy, traffic impact, and step-by-step CLI commands.
tags:
- ran
- ret
- cascaded-ret
- activation
- cli
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T07:59:47+00:00'
  source_sha256: 0d38afc99fc3972b
sources:
- title: Cascaded RET Support
  resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
---

This section outlines the procedure for activating the Cascaded RET Support feature, including traffic impact considerations, preconditions, rollout recommendations, and step-by-step CLI commands.

## Overview and Impact

* **Traffic Impact:** None. The feature operates entirely on the antenna control bus; user-plane traffic and cell availability are unaffected. Tilt changes performed after activation alter coverage by design, but activation and discovery themselves move no motors.
* **Preconditions:**
  * License key `FAK-51020` installed.
  * Baseline RET Support feature activated.
  * Site installation records confirming which ports carry cascades.
* **Recommended Rollout Strategy:** Activate node-wide, run a manual scan on one known multi-RET site, verify all expected `RetDevice` instances appear with correct serial numbers, then enable automatic scanning fleet-wide.

## Step-by-Step Activation Procedure

1. **Verify the license key is installed**
   ```bash
   rancli get NodeRoot=1,EquipmentFunction=1,FeatureCtrl=CascadedRetSupport licenseState
   ```
   *Expected result:* `licenseState=ENABLED` (key `FAK-51020`)

2. **Activate the feature**
   ```bash
   rancli set NodeRoot=1,EquipmentFunction=1,FeatureCtrl=CascadedRetSupport featureState=ACTIVATED
   ```

3. **Scan the AISG bus on the target radio unit port**
   ```bash
   rancli action NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1 scanAisgBus
   ```

4. **Calibrate a discovered secondary actuator**
   ```bash
   rancli action NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1,RetDevice=2 calibrate
   ```
   *Note:* Calibration is mandatory before issuing any tilt command.

5. **Verify discovery and calibration state**
   ```bash
   rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1 aldCount
   rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1,RetDevice=2 calibrationStatus
   ```

After activation, confirm that `ctrAldSupervisedTime` increments for every expected device.

# Cross-References

* [Feature Dependencies](feature-depedencies.md)
* [Deactivation Procedure](deactivation-procedure.md)
