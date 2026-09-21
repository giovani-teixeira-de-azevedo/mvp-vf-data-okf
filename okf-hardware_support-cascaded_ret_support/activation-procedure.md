---
type: procedure
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Detailed steps to verify, activate, scan, calibrate, and verify the Cascaded
  RET Support feature.
tags:
- RET
- Cascaded RET
- AISG
- CLI
- Activation
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:42:43+00:00'
  source_sha256: 0d38afc99fc3972b
sources:
- title: Cascaded RET Support
  resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
---

This section provides a step-by-step procedure to activate and verify the Cascaded RET Support feature. It covers traffic impact, pre-activation requirements, and the specific RAN CLI commands used for activation, scanning, calibration, and state verification.

## Traffic Impact

There is **no traffic impact** associated with this procedure. The Cascaded RET Support feature operates entirely on the antenna control bus; user-plane traffic and cell availability remain completely unaffected. While tilt changes performed after activation will alter coverage by design, the activation and discovery processes themselves do not move any motors.

## Preconditions and Recommended Rollout

### Preconditions
* The license key `FAK-51020` must be installed.
* The baseline RET Support feature must be activated.
* Site installation records must confirm which ports carry cascades.

### Recommended Rollout Strategy
1. Activate the feature node-wide.
2. Run a manual scan on one known multi-RET site.
3. Verify all expected `RetDevice` instances appear with the correct serial numbers.
4. Enable automatic scanning fleet-wide.

---

## Step-by-Step Activation Procedure

The activation process consists of five main steps:
1. Verify the license key.
2. Activate the feature control.
3. Trigger a bus scan on the target port.
4. Calibrate the newly discovered actuator (mandatory before any tilt command).
5. Verify the discovery and calibration state.

After activation is complete, confirm that the `ctrAldSupervisedTime` counter increments for every expected device.

### Step 1: Verify the License Key is Installed

Execute the following command to check the license state:

```bash
rancli get NodeRoot=1,EquipmentFunction=1,FeatureCtrl=CascadedRetSupport licenseState
```

**Expected Output:**
* `licenseState=ENABLED` (key `FAK-51020`)

### Step 2: Activate the Feature

Enable the Cascaded RET Support feature state:

```bash
rancli set NodeRoot=1,EquipmentFunction=1,FeatureCtrl=CascadedRetSupport featureState=ACTIVATED
```

### Step 3: Scan the AISG Bus on the Target Radio Unit Port

Trigger a scan on the desired radio unit port to discover connected devices:

```bash
rancli action NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1 scanAisgBus
```

### Step 4: Calibrate a Discovered Secondary Actuator

Calibration is mandatory for any newly discovered actuator before any tilt commands can be successfully issued:

```bash
rancli action NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1,RetDevice=2 calibrate
```

### Step 5: Verify Discovery and Calibration State

Check the number of discovered ALDs and the calibration status of the specific RET device:

```bash
rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1 aldCount
rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1,RetDevice=2 calibrationStatus
```

Additionally, verify that the supervised time counter `ctrAldSupervisedTime` is actively incrementing.

# Cross-References

* [Feature Dependencies](feature-depedencies.md) - For licensing and baseline RET prerequisites
* [Parameters](parameters.md) - For configuration attributes like `featureState`
* [Performance Management](performance-management.md) - For details regarding `ctrAldSupervisedTime`
* [Deactivation Procedure](deactivation-procedure.md) - For reversing this activation process
