---
type: procedure
resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf#deactivation-procedure
title: Deactivation Procedure
description: Provides instructions and command syntax for deactivating the Cascaded
  RET Support feature and verifying device management state.
tags:
- ret
- cascaded-ret
- deactivation
- rancli
- aisg
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T12:42:44+00:00'
  source_sha256: a87c4fd9405cf1ce
sources:
- title: Cascaded RET Support
  resource: data/vodafone-mvp/raw/Cascaded RET Support.pdf
---

This section describes the procedure for deactivating the Cascaded RET Support feature, including operational impact, prerequisites, and step-by-step execution using `rancli`.

## Operational Impact and Considerations

- **Traffic impact:** None.
- **Actuator state:** Deactivation removes O&M visibility of secondary cascade devices but does not move any actuator; all arrays remain at their last commanded tilt.
- **Primary devices:** The first device on each bus remains manageable through the baseline RET Support feature.
- **Prerequisite:** Deactivate only after confirming no tilt optimization campaign is in progress, since automation functions lose access to secondary actuators immediately.
- **Configuration retention:** Secondary `RetDevice` instances transition to an unmanaged state and are retained in the configuration for fast re-activation.
- **Licensing:** The license key may remain installed.

## Step-by-Step Procedure

1. **Deactivate the feature:**

   ```bash
   rancli set NodeRoot=1,EquipmentFunction=1,FeatureCtrl=CascadedRetSupport featureState=DEACTIVATED
   ```

2. **Verify device management state:**

   ```bash
   rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1 aldCount
   rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1,AisgPort=1,RetDevice=2 operationalState
   ```

# Cross-References

- [Activation Procedure](activation-procedure.md)
- [Feature Overview](feature-overview.md)
