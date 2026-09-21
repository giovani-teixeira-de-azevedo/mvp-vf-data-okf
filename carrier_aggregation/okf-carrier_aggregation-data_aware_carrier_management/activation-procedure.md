---
type: procedure
resource: data/vodafone-mvp/raw/Data-Aware Carrier Management.pdf#activation-procedure
title: Activation Procedure
description: Detailed step-by-step procedure for activating and tuning the Data-Aware
  Carrier Management feature.
tags:
- activation
- configuration
- rancli
- carrier-management
- s-cell
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:37:40+00:00'
  source_sha256: a0ddcfc0ca37db02
sources:
- title: Data-Aware Carrier Management
  resource: data/vodafone-mvp/raw/Data-Aware Carrier Management.pdf
---

The **Activation Procedure** outlines the necessary prerequisites, recommended rollout strategy, and step-by-step CLI commands required to enable and configure the Data-Aware Carrier Management feature.

### Impact and Preconditions

- **Traffic Impact:** None. Activation changes only future SCell configuration decisions; existing UE configurations are untouched until their next natural reconfiguration. No cell lock or maintenance window is required.
- **Preconditions:**
  - License key `FAK-33012` must be installed.
  - NR DL Carrier Aggregation must be active and carrying traffic.
  - A Performance Management (PM) baseline must be collected (see [Performance Management](performance-management.md)).
- **Recommended Rollout Order:**
  1. Activate on one representative node with default settings.
  2. Verify after 48 hours that Promotion Latency P95 is under 60 ms and high-percentile burst throughput is unchanged.
  3. Roll out cluster by cluster.

---

### Step-by-Step Execution

The activation process consists of verifying the license, activating the feature control, enabling the function along with tuning parameters, and final verification.

#### Step 1: Verify the License Key is Installed
Execute the following CLI command to ensure the required license key is present and enabled:

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=DataAwareCarrierMgmt licenseState
```

*Expected Output:*
```text
licenseState=ENABLED (key FAK-33012)
```

#### Step 2: Activate the Feature
Activate the feature control state on the node:

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=DataAwareCarrierMgmt featureState=ACTIVATED
```

#### Step 3: Enable and Tune the Feature
Enable the function and explicitly set the two headline threshold parameters (see [Parameters](parameters.md) for detailed descriptions of these parameters):

```bash
rancli set NodeRoot=1,NrFunction=1 dataAwareCmEnabled=true
rancli set NodeRoot=1,NrFunction=1 bulkBurstThr=500 interactiveBurstThr=50
```

#### Step 4: Verify Activation
Verify that the configuration has been applied successfully:

```bash
rancli get NodeRoot=1,NrFunction=1 dataAwareCmEnabled
```

*Post-Change Validation:* Confirm that the counter `ctrScellConfigSuppressed` begins to increment while downlink throughput KPIs remain stable (see [Performance Management](performance-management.md)).

# Cross-References

* [Deactivation Procedure](deactivation-procedure.md)
* [Feature Dependencies](feature-depedencies.md)
* [Feature Operation](feature-operation.md)
* [Parameters](parameters.md)
* [Performance Management](performance-management.md)
