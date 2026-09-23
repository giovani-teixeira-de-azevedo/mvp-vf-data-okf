---
type: procedure
resource: data/vodafone-mvp/raw/Energy Metering.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Step-by-step procedure for activating the Energy Metering feature, including
  license verification, activation commands, metering method checks, and post-activation
  validation.
tags:
- energy-metering
- activation
- procedure
- rancli
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T10:26:30+00:00'
  source_sha256: 08e45ac3ac5815b2
sources:
- title: Energy Metering
  resource: data/vodafone-mvp/raw/Energy Metering.pdf
---

This document details the activation procedure for the Energy Metering feature, outlining impact considerations, prerequisites, CLI execution steps, and verification actions.

## Overview and Prerequisites

- **Traffic Impact**: None. The feature only reads measurement circuitry and computes estimates; no radio or transport behavior changes. It can be activated at any time of day across the whole fleet.
- **Preconditions**: License key `FAK-51030` installed. No dependent features are required.
- **Rollout Recommendation**: Fleet-wide in one campaign, since the value of energy data grows with coverage. PM collection should begin immediately so that the pre/post baseline for any future energy-saving feature is in place.

## Step-by-Step Procedure

### Step 1: Verify License Key Installation
Verify that the required license key is installed prior to activation:

```bash
rancli get NodeRoot=1,EquipmentFunction=1,FeatureCtrl=EnergyMetering licenseState
```

**Expected output**: `licenseState=ENABLED` (key `FAK-51030`).

### Step 2: Activate the Feature
Enable the feature state on the node:

```bash
rancli set NodeRoot=1,EquipmentFunction=1,FeatureCtrl=EnergyMetering featureState=ACTIVATED
```

### Step 3: Check Per-Unit Metering Method
Confirm the metering method (`measured` versus `estimated`) for each unit so that data consumers know the accuracy class of each site:

```bash
rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1 meteringMethod
rancli get NodeRoot=1,EquipmentFunction=1,BasebandUnit=BB1 meteringMethod
```

### Step 4: Spot-Check Momentary Power
Take a momentary power spot reading as a sanity check against the site's rectifier display:

```bash
rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1 momentaryPower
```

## Post-Activation Verification

After the first full Result Output Period (ROP), verify that the counter `ctrNodeEnergyConsumed` is non-zero and consistent with the spot reading.

# Cross-References

- [Feature Dependencies](feature-depedencies.md)
- [Performance Management](performance-management.md)
- [Deactivation Procedure](deactivation-procedure.md)
