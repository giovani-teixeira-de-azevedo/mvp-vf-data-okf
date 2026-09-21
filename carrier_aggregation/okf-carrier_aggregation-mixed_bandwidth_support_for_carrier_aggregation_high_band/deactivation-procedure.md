---
type: procedure
resource: data/vodafone-mvp/raw/Mixed Bandwidth Support for Carrier Aggregation High-Band.pdf#deactivation-procedure
title: Deactivation Procedure
description: Step-by-step instructions and traffic impact details for deactivating
  the Mixed Bandwidth Support for Carrier Aggregation High-Band feature.
tags:
- deactivation
- rancli
- mixed-bandwidth
- carrier-aggregation
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:38:30+00:00'
  source_sha256: ef58e32df3199d5b
sources:
- title: Mixed Bandwidth Support for Carrier Aggregation High-
  resource: data/vodafone-mvp/raw/Mixed Bandwidth Support for Carrier Aggregation
    High-Band.pdf
---

This section describes the procedure to deactivate the Mixed Bandwidth Support for Carrier Aggregation High-Band feature. It includes the steps required to disable the function, deactivate the feature control, and verify the deactivation status, as well as the anticipated impact on traffic.

## Traffic Impact and Considerations

Deactivating the feature leads to a **gradual capacity reduction**:
- **Immediate Effect:** Deactivation immediately prevents any new mixed-bandwidth configurations.
- **Active UEs:** UEs currently holding a mixed-bandwidth combination keep it until their next reconfiguration, at which point the odd-sized Component Carriers (CCs) are dropped from their Carrier Aggregation (CA) set.
- **Capacity Loss:** The aggregate capacity contributed by those odd-sized CCs is lost. Deactivation should be planned outside the busy hour on loaded sites.
- **Carrier Status:** The odd-sized carriers remain on air serving single-carrier traffic. If they are to be refarmed, they must be removed separately.

## Deactivation Steps

Deactivation is executed in three steps: disabling the function, deactivating the feature control, and verifying the state.

### Step 1: Disable the Function
Execute the following command to set the parameter `mixedBwCaEnabled` to `false`:

```bash
rancli set NodeRoot=1,NrFunction=1 mixedBwCaEnabled=false
```

### Step 2: Deactivate the Feature
Deactivate the feature control by setting the `featureState` of `MixedBwCaHighBand` to `DEACTIVATED`:

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=MixedBwCaHighBand featureState=DEACTIVATED
```

### Step 3: Verify
Verify that the feature state is deactivated:

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=MixedBwCaHighBand featureState
```

# Cross-References

* [Activation Procedure](activation-procedure.md)
* [Feature Overview](feature-overview.md)
* [Parameters](parameters.md)
