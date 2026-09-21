---
type: procedure
resource: data/vodafone-mvp/raw/Data-Aware Carrier Management.pdf#deactivation-procedure
title: Deactivation Procedure
description: Deactivation procedure for the Data-Aware Carrier Management feature,
  including disabling the function, deactivating the feature control, and verification.
tags:
- Data-Aware Carrier Management
- Deactivation Procedure
- RAN
- SCell
- rancli
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:37:36+00:00'
  source_sha256: 67637336b1965349
sources:
- title: Data-Aware Carrier Management
  resource: data/vodafone-mvp/raw/Data-Aware Carrier Management.pdf
---

This section describes the step-by-step procedure to deactivate the Data-Aware Carrier Management feature, including its traffic impact and verification steps.

## Traffic Impact

Deactivating this feature has **no traffic impact**. Upon deactivation:
- The node reverts to static Carrier Aggregation (CA) behavior.
- CA-capable UEs are configured with all candidate Secondary Cells (SCells) at their next reconfiguration.
- Connected UEs are not force-reconfigured, meaning the transition happens gradually and transparently.
- RRC reconfiguration volume and SCell activation counts are expected to return to baseline within an hour as the connected-UE population turns over.

## Step-by-Step Deactivation Procedure

The deactivation process consists of three steps: disabling the estimator function, deactivating the feature control, and verifying the feature state.

### Step 1: Disable the Function
Disable the function so the estimator stops influencing decisions:

```bash
rancli set NodeRoot=1,NrFunction=1 dataAwareCmEnabled=false
```

### Step 2: Deactivate the Feature
Deactivate the feature control:

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=DataAwareCarrierMgmt featureState=DEACTIVATED
```

### Step 3: Verify
Verify that the feature state has been successfully set to `DEACTIVATED`:

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=DataAwareCarrierMgmt featureState
```

# Cross-References
* [Activation Procedure](activation-procedure.md) - Procedure to activate the Data-Aware Carrier Management feature.
