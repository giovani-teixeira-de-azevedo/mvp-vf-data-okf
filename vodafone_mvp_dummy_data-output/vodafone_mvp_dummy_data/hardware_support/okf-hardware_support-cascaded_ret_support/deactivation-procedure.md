---
type: procedure
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#deactivation-procedure
title: Deactivation Procedure
description: Step-by-step procedure for deactivating the Cascaded RET Support feature
  and verifying device management state.
tags:
- cascaded-ret
- ret
- deactivation
- rancli
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T15:10:26+00:00'
  source_sha256: a87c4fd9405cf1ce
sources:
- title: Cascaded RET Support
  resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
---

This document details the step-by-step procedure for deactivating the Cascaded RET Support feature and verifying the device management state within the node configuration.

## Prerequisites and Impact

- **Traffic Impact:** None.
- **Actuator Position:** Deactivation does not move any actuator; all arrays remain at their last commanded tilt.
- **Device Management & Visibility:** Deactivation removes O&M visibility of secondary cascade devices. The first device on each bus remains manageable through the baseline RET Support feature. Secondary `RetDevice` instances transition to an unmanaged state and are retained in the configuration for fast re-activation.
- **Prerequisites:** Deactivate only after confirming no tilt optimization campaign is in progress, since automation functions lose access to secondary actuators immediately.
- **Licensing:** The license key may remain installed.

## Deactivation Steps

### Step 1: Deactivate the feature

Deactivate the feature control using `rancli`:

```bash
rancli set NodeRoot=1,EquipmentFunction=1,FeatureCtrl=CascadedRetSupport featureState=DEACTIVATED
```

### Step 2: Verify device management state

Verify the ALD count and that secondary `RetDevice` instances have transitioned to an unmanaged state:

```bash
rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1 aldCount
rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1,RetDevice=2 operationalState
```

# Cross-References

- [Activation Procedure](activation-procedure.md)
