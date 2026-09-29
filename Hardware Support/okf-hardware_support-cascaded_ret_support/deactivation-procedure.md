---
type: procedure
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#deactivation-procedure
title: Deactivation Procedure
description: Deactivation procedure and CLI commands for the Cascaded RET Support
  feature.
tags:
- ret
- cascaded-ret
- deactivation
- rancli
- o-and-m
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-29T16:03:05+00:00'
  source_sha256: a87c4fd9405cf1ce
sources:
- title: Cascaded RET Support
  resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
---

This clause outlines the procedure and traffic impact for deactivating the Cascaded RET Support feature. It details the operational precautions and CLI commands required to disable feature control and verify the management state of secondary RET devices.

## Deactivation Overview and Impact

- **Traffic Impact:** None. Deactivation removes O&M visibility of secondary cascade devices but does not move any actuator; all arrays remain at their last commanded tilt.
- **Baseline Device Management:** The first device on each bus remains manageable through the baseline RET Support feature.
- **Operational Precautions:** Deactivate only after confirming no tilt optimization campaign is in progress, since automation functions lose access to secondary actuators immediately.
- **Configuration & Licensing:** Secondary `RetDevice` instances transition to an unmanaged state and are retained in the configuration for fast re-activation. The license key may remain installed.

## Procedure Steps

1. **Deactivate the feature control**
   ```bash
   rancli set NodeRoot=1,EquipmentFunction=1,FeatureCtrl=CascadedRetSupport featureState=DEACTIVATED
   ```

2. **Verify device management state**
   ```bash
   rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1 aldCount
   rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1,RetDevice=2 operationalState
   ```

## Cross-References

- [Activation Procedure](activation-procedure.md)
