---
type: procedure
resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Step-by-step procedure to verify licenses, activate, and configure the
  Dynamic Component Carrier Management feature.
tags:
- activation
- procedure
- dynamic-cc-management
- rancli
- configuration
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:37:58+00:00'
  source_sha256: 9c3401546223ae94
sources:
- title: Dynamic Component Carrier Management
  resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf
---

This section provides the step-by-step activation procedure for the Dynamic Component Carrier Management feature, including pre-activation checks, configuration tuning, and post-activation verification commands.

### Traffic Impact and Scheduling
* **Traffic Impact**: None. 
* **Signaling and Locking**: New SCell setups immediately follow dynamic scoring. Existing SCell configurations are modified only by paced rebalancing, preventing signaling bursts or cell locks.
* **Scheduling**: Activation can safely proceed during business hours, though it is recommended to monitor the first busy hour post-activation.

### Preconditions
Before starting the activation procedure, ensure that:
* The license key **FAK-33013** is installed.
* Baseline Carrier Aggregation (CA) features are active.
* Carrier relations are defined for all co-sited carriers.
* Performance Management (PM) baseline data has been collected.

### Recommended Rollout Strategy
1. Enable the feature on a pilot site with a conservative rebalancing threshold (`rebalanceThr=90`).
2. Verify Migration Productivity.
3. Gradually lower the threshold toward the default value of `80` and expand the rollout.

---

### Step-by-Step Activation

#### Step 1: Verify License Key Installation
Execute the following command to verify that the required license key is installed:

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=DynamicCcMgmt licenseState
```

**Expected Output:**
```text
licenseState=ENABLED (key FAK-33013)
```

#### Step 2: Activate the Feature Control
Activate the feature state in the configuration tree:

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=DynamicCcMgmt featureState=ACTIVATED
```

#### Step 3: Enable and Tune the Feature Parameters
Enable the manager, and explicitly set the weights and thresholds:

```bash
# Enable the manager
rancli set NodeRoot=1,NrFunction=1 dynamicCcMgmtEnabled=true

# Set optimization weights (Load, RSRP, and Throughput)
rancli set NodeRoot=1,NrFunction=1 loadWeight=50 rsrpWeight=30 tputWeight=20

# Configure the rebalancing threshold and pacing limit
rancli set NodeRoot=1,NrFunction=1 rebalanceThr=80 maxMigrationsPerRop=100
```

#### Step 4: Verify Enablement
Confirm that the dynamic Component Carrier management feature is active:

```bash
rancli get NodeRoot=1,NrFunction=1 dynamicCcMgmtEnabled
```

---

### Post-Activation Monitoring
After activation, verify the following metrics over the first few days:
* Confirm that the counter `ctrScellSetups` is incrementing.
* Monitor the `Load Spread` metric to ensure a downward trend.

# Cross-References
* [Feature Dependencies](feature-depedencies.md) — Details on the required license key FAK-33013 and baseline CA configurations.
* [Parameters](parameters.md) — Descriptions and default values for `loadWeight`, `rsrpWeight`, `tputWeight`, `rebalanceThr`, and `maxMigrationsPerRop`.
* [Performance Management](performance-management.md) — Verification details for `ctrScellSetups` and `Load Spread` metrics.
* [Deactivation Procedure](deactivation-procedure.md) — Steps to roll back or disable the feature if needed.
