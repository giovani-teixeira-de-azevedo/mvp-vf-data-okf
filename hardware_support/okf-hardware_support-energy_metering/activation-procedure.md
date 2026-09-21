---
type: procedure
resource: data/vodafone-mvp/raw/Energy Metering.pdf#activation-procedure
title: Activation Procedure
description: Step-by-step activation procedure, license verification, and initial
  sanity checks for the Energy Metering feature.
tags:
- activation-procedure
- energy-metering
- rancli
- licensing
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:43:09+00:00'
  source_sha256: 08e45ac3ac5815b2
sources:
- title: Energy Metering
  resource: data/vodafone-mvp/raw/Energy Metering.pdf
---

This document describes the step-by-step activation procedure for the Energy Metering feature. The procedure outlines the license verification, feature enablement, hardware capability checks, and post-activation spot checks.

## Traffic Impact and Rollout Strategy

* **Traffic Impact:** None. The feature only reads measurement circuitry and computes estimates, resulting in no changes to radio or transport behavior. It can be safely activated at any time of day across the entire fleet.
* **Preconditions:** License key `FAK-51030` must be installed. There are no dependent feature requirements.
* **Rollout Recommendation:** It is recommended to deploy this feature fleet-wide in a single campaign. Because the value of energy data grows with coverage, beginning PM collection immediately establishes a pre- and post-activation baseline for any future energy-saving features.

## Step-by-Step Activation

The activation process consists of verifying the license, enabling the feature, confirming the unit-specific metering methods, and validating current power draws.

### Step 1: Verify License Key Installation

Verify that the required license key `FAK-51030` is installed and enabled.

```bash
rancli get NodeRoot=1,EquipmentFunction=1,FeatureCtrl=EnergyMetering licenseState
```

**Expected Output:**
* `licenseState=ENABLED`

### Step 2: Activate the Feature

Enable the operational state of the Energy Metering feature.

```bash
rancli set NodeRoot=1,EquipmentFunction=1,FeatureCtrl=EnergyMetering featureState=ACTIVATED
```

### Step 3: Check Unit Metering Method

Confirm the metering method (measured versus estimated) for each active unit. This step ensures that data consumers are aware of the accuracy class of each site.

```bash
rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1 meteringMethod
rancli get NodeRoot=1,EquipmentFunction=1,BasebandUnit=BB1 meteringMethod
```

### Step 4: Spot-Check Momentary Power

Take a momentary power spot reading as a sanity check against the site's physical rectifier display.

```bash
rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1 momentaryPower
```

## Post-Activation Verification

After the first full Reporting Output Period (ROP), verify that the cumulative energy consumption counter is recording data properly:
* Ensure `ctrNodeEnergyConsumed` is non-zero.
* Confirm that the recorded value is consistent with the momentary power spot reading taken in Step 4.

# Cross-References

* [Feature Overview](feature-overview.md)
* [Feature Dependencies](feature-depedencies.md)
* [Parameters](parameters.md)
* [Performance Management](performance-management.md)
* [Deactivation Procedure](deactivation-procedure.md)
