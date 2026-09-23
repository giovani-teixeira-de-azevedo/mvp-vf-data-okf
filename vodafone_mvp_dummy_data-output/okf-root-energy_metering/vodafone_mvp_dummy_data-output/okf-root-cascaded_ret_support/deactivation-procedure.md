---
type: procedure
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#deactivation-procedure
title: Deactivation Procedure
description: Outlines the operational impact, considerations, and step-by-step CLI
  commands for deactivating the Cascaded RET Support feature.
tags:
- cascaded-ret
- deactivation
- rancli
- ret-device
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T10:08:32+00:00'
  source_sha256: a87c4fd9405cf1ce
sources:
- title: Cascaded RET Support
  resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
---

This clause provides the procedure for deactivating the Cascaded RET Support feature, detailing traffic impact, operational considerations, and verification steps.

## Operational Impact and Considerations

- **Traffic Impact:** None.
- **Actuator Position:** Deactivation removes O&M visibility of secondary cascade devices but does not move any actuator; all arrays remain at their last commanded tilt.
- **Baseline RET Support:** The first device on each bus remains manageable through the baseline RET Support feature.
- **Pre-deactivation Requirement:** Deactivate only after confirming no tilt optimization campaign is in progress, since automation functions lose access to secondary actuators immediately.
- **Configuration and License:** Secondary `RetDevice` instances transition to an unmanaged state and are retained in the configuration for fast re-activation. The license key may remain installed.

## Step-by-Step Procedure

1. **Deactivate the feature**
   ```bash
   rancli set NodeRoot=1,EquipmentFunction=1,FeatureCtrl=CascadedRetSupport featureState=DEACTIVATED
   ```

2. **Verify device management state**
   ```bash
   rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1 aldCount
   rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1,RetDevice=2 operationalState
   ```

# Cross-References

- [Activation Procedure](activation-procedure.md)
