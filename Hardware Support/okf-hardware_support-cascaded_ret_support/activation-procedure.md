---
type: procedure
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Step-by-step activation procedure, preconditions, and verification commands
  for Cascaded RET Support.
tags:
- cascaded-ret
- activation
- rancli
- aisg
- calibration
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-29T16:02:57+00:00'
  source_sha256: 0d38afc99fc3972b
sources:
- title: Cascaded RET Support
  resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
---

This section details the preconditions, recommended rollout strategy, traffic impact, and step-by-step execution procedure for activating the Cascaded RET Support feature.

## Overview and Preconditions

* **Traffic Impact:** None. The feature operates entirely on the antenna control bus; user-plane traffic and cell availability are unaffected. Tilt changes performed after activation alter coverage by design, but activation and discovery themselves move no motors.
* **Preconditions:**
  * License key FAK-51020 installed.
  * Baseline RET Support feature activated.
  * Site installation records confirming which ports carry cascades.
* **Recommended Rollout:** Activate node-wide, run a manual scan on one known multi-RET site, verify all expected `RetDevice` instances appear with correct serial numbers, then enable automatic scanning fleet-wide.

## Step-by-Step Procedure

Step 1 verifies the license; step 2 activates the feature control; step 3 triggers a bus scan on the target port; step 4 calibrates a newly discovered actuator (mandatory before any tilt command); step 5 verifies discovery and calibration state. After activation, confirm `ctrAldSupervisedTime` increments for every expected device.

### 1. Verify the License Key is Installed

```bash
rancli get NodeRoot=1,EquipmentFunction=1,FeatureCtrl=CascadedRetSupport licenseState
```
*Expected:* `licenseState=ENABLED` (key FAK-51020)

### 2. Activate the Feature

```bash
rancli set NodeRoot=1,EquipmentFunction=1,FeatureCtrl=CascadedRetSupport featureState=ACTIVATED
```

### 3. Scan the AISG Bus on the Target Radio Unit Port

```bash
rancli action NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1 scanAisgBus
```

### 4. Calibrate a Discovered Secondary Actuator

```bash
rancli action NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1,RetDevice=2 calibrate
```

### 5. Verify

```bash
rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1 aldCount
rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1,RetDevice=2 calibrationStatus
```

# Cross-References

* [Deactivation Procedure](deactivation-procedure.md)
* [Feature Dependencies](feature-depedencies.md)
