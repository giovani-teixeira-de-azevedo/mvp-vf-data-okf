---
type: procedure
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#deactivation-procedure
title: Deactivation Procedure
description: Deactivation procedure for the Cascaded RET Support feature, including
  configuration verification commands.
tags:
- cascaded-ret
- deactivation
- rancli
- ret-device
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:42:49+00:00'
  source_sha256: a87c4fd9405cf1ce
sources:
- title: Cascaded RET Support
  resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
---

The Deactivation Procedure outlines the steps to deactivate the Cascaded RET Support feature on a node and verify the resulting management state of the secondary RET devices. This procedure ensures that secondary devices transition to an unmanaged state while preserving their existing configurations for fast re-activation.

## Pre-Deactivation Considerations

* **Traffic Impact:** None.
* **Actuator Position:** Actuators do not move; all arrays remain at their last commanded tilt.
* **First Device Manageability:** The first device on each AISG bus remains manageable through the baseline RET Support feature.
* **Automation and Optimization:** Deactivate only after confirming that no tilt optimization campaign is in progress. Automation functions lose access to secondary actuators immediately upon deactivation.
* **Configuration Retention:** Secondary `RetDevice` instances transition to an unmanaged state but are retained in the configuration to allow fast re-activation.
* **License Key:** The license key may remain installed on the system.

## Step-by-Step Procedure

### 1. Deactivate the Feature
Disable the feature control state using the following command:

```bash
rancli set NodeRoot=1,EquipmentFunction=1,FeatureCtrl=CascadedRetSupport featureState=DEACTIVATED
```

### 2. Verify Device Management State
Verify the Antenna Line Device (ALD) count and check the operational state of the secondary `RetDevice` instance:

```bash
rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1 aldCount
rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1,RetDevice=2 operationalState
```

# Cross-References

* [Feature Overview](feature-overview.md)
* [Network Impact](network-impact.md)
* [Activation Procedure](activation-procedure.md)
