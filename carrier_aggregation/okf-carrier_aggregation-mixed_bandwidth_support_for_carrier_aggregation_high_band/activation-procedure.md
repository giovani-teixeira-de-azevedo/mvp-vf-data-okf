---
type: procedure
resource: data/vodafone-mvp/raw/Mixed Bandwidth Support for Carrier Aggregation High-Band.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Step-by-step procedure to activate Mixed Bandwidth Support for Carrier
  Aggregation High-Band, including license verification and configuration steps.
tags:
- activation
- mixed-bandwidth
- carrier-aggregation
- cli-commands
- gnodeb
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:38:31+00:00'
  source_sha256: c16f5b81fea6ee15
sources:
- title: Mixed Bandwidth Support for Carrier Aggregation High-
  resource: data/vodafone-mvp/raw/Mixed Bandwidth Support for Carrier Aggregation
    High-Band.pdf
---

This section outlines the step-by-step activation procedure for the Mixed Bandwidth Support for Carrier Aggregation High-Band feature. 

Enabling this feature has **no traffic impact**, as it only widens the set of combinations the builder can select for future CA configurations. No existing configuration is altered, and no cell restart is required. Note that the odd-sized carriers themselves must already be on air, which is a separate carrier build activity.

### Preconditions

Before proceeding with the activation steps, ensure the following conditions are met:
* License key **FAK-33015** is installed.
* The **FR2 CA baseline** is active.
* The non-uniform carriers are configured and unlocked.

#### Rollout Recommendation
It is recommended to enable the feature first on the nodes covering the irregular holding, monitor the *Combo Downgrade Rate* for one week to catch any device-capability issues, and then leave it enabled network-wide.

---

### Step-by-Step Activation

#### Step 1: Verify the License Key Installation
Verify that the license key `FAK-33015` is installed and enabled.

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=MixedBwCaHighBand licenseState
```
**Expected Output:**
`licenseState=ENABLED`

#### Step 2: Activate the Feature
Activate the feature control state.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=MixedBwCaHighBand featureState=ACTIVATED
```

#### Step 3: Enable the Feature and Set Policies
Enable the mixed bandwidth CA function, set the minimum secondary cell (SCell) bandwidth, and configure the primary cell (PCell) policy.

```bash
rancli set NodeRoot=1,NrFunction=1 mixedBwCaEnabled=true
rancli set NodeRoot=1,NrFunction=1 minScellBandwidth=50 smallCcPcellAllowed=false
```

#### Step 4: Verify Feature Activation
Confirm that the feature is successfully enabled on the node.

```bash
rancli get NodeRoot=1,NrFunction=1 mixedBwCaEnabled
```

### Post-Activation Verification
After activation, verify that the performance counter `ctrMixedBwConfigs` increments as CA-capable user equipments (UEs) reconnect.

# Cross-References

* [Feature Dependencies](feature-depedencies.md) - For details regarding the required licenses and baseline configurations.
* [Parameters](parameters.md) - Explains parameters like `mixedBwCaEnabled`, `minScellBandwidth`, and `smallCcPcellAllowed`.
* [Performance Management](performance-management.md) - Information on monitoring counters like `ctrMixedBwConfigs` and performance metrics.
* [Deactivation Procedure](deactivation-procedure.md) - Instructions on how to roll back or deactivate the feature.
