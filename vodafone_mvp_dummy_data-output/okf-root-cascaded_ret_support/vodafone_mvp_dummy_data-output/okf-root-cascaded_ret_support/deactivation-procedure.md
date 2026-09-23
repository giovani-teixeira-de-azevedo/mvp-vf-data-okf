---
type: procedure
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#deactivation-procedure
title: Deactivation Procedure
description: Instructions and CLI commands for deactivating the Cascaded RET Support
  feature and verifying device management state.
tags:
- cascaded-ret
- deactivation
- rancli
- ret-support
- o-and-m
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T07:59:46+00:00'
  source_sha256: a87c4fd9405cf1ce
sources:
- title: Cascaded RET Support
  resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
---

This section outlines the process for deactivating the Cascaded RET Support feature, detailing traffic impact, operational considerations, and step-by-step CLI commands.

## Operational Impact and Considerations

* **Traffic Impact:** None.
* **Actuator Position:** Deactivation does not move any actuator; all arrays remain at their last commanded tilt.
* **O&M Visibility:** Deactivation removes O&M visibility of secondary cascade devices. The first device on each bus remains manageable through the baseline RET Support feature.
* **Automation Functions:** Deactivate only after confirming no tilt optimization campaign is in progress, as automation functions lose access to secondary actuators immediately.
* **Configuration & License:** Secondary `RetDevice` instances transition to an unmanaged state but are retained in the configuration for fast re-activation. The license key may remain installed.

## Step-by-Step Deactivation Procedure

### Step 1: Deactivate the feature

Deactivate feature control using `rancli`:

```bash
rancli set NodeRoot=1,EquipmentFunction=1,FeatureCtrl=CascadedRetSupport featureState=DEACTIVATED
```

### Step 2: Verify device management state

Verify device management state and operational state using `rancli`:

```bash
rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1 aldCount
rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1,RetDevice=2 operationalState
```

# Cross-References

* [Activation Procedure](activation-procedure.md)
* [Feature Overview](feature-overview.md)
