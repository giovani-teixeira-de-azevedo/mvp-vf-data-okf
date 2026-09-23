---
type: procedure
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#deactivation-procedure
title: DEACTIVATION PROCEDURE
description: Step-by-step procedure and operational considerations for deactivating
  the Cascaded RET Support feature.
tags:
- cascaded-ret
- deactivation
- rancli
- ret-device
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T18:53:15+00:00'
  source_sha256: a87c4fd9405cf1ce
sources:
- title: Cascaded RET Support
  resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
---

This document details the step-by-step deactivation procedure and operational impacts for the Cascaded RET Support feature in the node configuration.

## Operational Considerations and Traffic Impact

* **Traffic Impact:** None.
* **Actuator Position:** Deactivation removes O&M visibility of secondary cascade devices but does not move any actuator; all arrays remain at their last commanded tilt.
* **Baseline Management:** The first device on each bus remains manageable through the baseline RET Support feature.
* **Prerequisites:** Deactivate only after confirming no tilt optimization campaign is in progress, since automation functions lose access to secondary actuators immediately.
* **Licensing & Configuration State:** Secondary `RetDevice` instances transition to an unmanaged state but are retained in the configuration for fast re-activation. The license key may remain installed.

## Deactivation Procedure

### Step 1: Deactivate the feature

```bash
rancli set NodeRoot=1,EquipmentFunction=1,FeatureCtrl=CascadedRetSupport featureState=DEACTIVATED
```

### Step 2: Verify device management state

```bash
rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1 aldCount
rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1,RetDevice=2 operationalState
```

# Cross-References

* [ACTIVATION PROCEDURE](activation-procedure.md)
