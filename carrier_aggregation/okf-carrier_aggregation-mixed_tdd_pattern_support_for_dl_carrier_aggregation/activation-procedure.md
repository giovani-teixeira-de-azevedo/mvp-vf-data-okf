---
type: procedure
resource: data/vodafone-mvp/raw/Mixed TDD Pattern Support for DL Carrier Aggregation.pdf#activation-procedure
title: Activation Procedure
description: Step-by-step procedure to verify licenses, configure parameters via rancli,
  and activate Mixed TDD Pattern Support for DL Carrier Aggregation.
tags:
- rancli
- activation
- carrier-aggregation
- tdd-pattern
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:38:52+00:00'
  source_sha256: dfd050cd263acc7d
sources:
- title: Mixed TDD Pattern Support for DL Carrier Aggregation
  resource: data/vodafone-mvp/raw/Mixed TDD Pattern Support for DL Carrier Aggregation.pdf
---

This document describes the step-by-step activation procedure for the Mixed TDD Pattern Support for DL Carrier Aggregation feature. It outlines the traffic impact, preconditions, recommended rollout strategy, configuration commands, and verification steps.

## Pre-Activation Details

### Traffic Impact
* **Traffic Impact**: None. The feature only widens the combination space for future Carrier Aggregation (CA) configurations; existing configurations and the TDD patterns themselves are untouched.
* **Service Interruption**: No cell restart or maintenance window is required. (Note: Changing a carrier's TDD pattern is a separate, service-affecting activity outside the scope of this feature.)

### Preconditions
Before starting the activation procedure, verify the following conditions are met:
1. License key **FAK-33016** is installed on the node.
2. CA baseline is active (see [Feature Dependencies](feature-depedencies.md)).
3. Phase synchronization is verified across all aggregated carriers.
4. PCell PUCCH utilization is baselined.

### Recommended Rollout Strategy
1. Enable the feature on a single site pair with the mixed-pattern carriers.
2. Verify **HARQ RTT Delta** and **Codebook Mismatch** for a duration of 48 hours.
3. Expand rollout across the target cluster if no anomalies are observed.

---

## Step-by-Step Activation Procedure

### Step 1: Verify the License Key
Execute the following command to check if the license key is installed and enabled:

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=MixedTddPatternCa licenseState
```

**Expected Output:**
```text
licenseState=ENABLED
```
*(Indicates license key FAK-33016 is valid and enabled)*

### Step 2: Activate the Feature Control
Activate the feature control state on the node:

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=MixedTddPatternCa featureState=ACTIVATED
```

### Step 3: Enable and Tune the Feature
Enable the feature function and configure the tuning parameters on the `NrFunction` level:

```bash
rancli set NodeRoot=1,NrFunction=1 mixedTddCaEnabled=true
rancli set NodeRoot=1,NrFunction=1 maxPatternRatio=2 harqCodebookMode=TYPE2 pucchLoadGuard=85
```

*For more information on these configuration settings, refer to [Parameters](parameters.md).*

### Step 4: Verify Enablement
Confirm that the main feature enable switch is active:

```bash
rancli get NodeRoot=1,NrFunction=1 mixedTddCaEnabled
```

**Expected Output:**
```text
mixedTddCaEnabled=true
```

---

## Post-Activation Verification
After enabling the feature, confirm that the performance counter `ctrMixedCaConfigs` increments as capable UEs reconnect to the network. Detailed performance monitoring instructions and counter information can be found in [Performance Management](performance-management.md).

If rollback is necessary, follow the steps in [Deactivation Procedure](deactivation-procedure.md).

# Cross-References
* [Feature Dependencies](feature-depedencies.md)
* [Parameters](parameters.md)
* [Performance Management](performance-management.md)
* [Deactivation Procedure](deactivation-procedure.md)
