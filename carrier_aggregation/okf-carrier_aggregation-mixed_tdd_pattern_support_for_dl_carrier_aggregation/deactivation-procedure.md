---
type: procedure
resource: data/vodafone-mvp/raw/Mixed TDD Pattern Support for DL Carrier Aggregation.pdf#deactivation-procedure
title: Deactivation Procedure
description: Step-by-step process and traffic impact of deactivating the Mixed TDD
  Pattern Support for DL Carrier Aggregation feature.
tags:
- deactivation
- procedure
- RAN
- CLI
- carrier aggregation
- TDD
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:38:57+00:00'
  source_sha256: d5e96c0f6b9b07c3
sources:
- title: Mixed TDD Pattern Support for DL Carrier Aggregation
  resource: data/vodafone-mvp/raw/Mixed TDD Pattern Support for DL Carrier Aggregation.pdf
---

This section outlines the step-by-step procedure to deactivate the Mixed TDD Pattern Support for DL Carrier Aggregation feature, along with the associated traffic impacts.

## Traffic Impact and Considerations

Before deactivating the feature, consider the following traffic impacts:
- **Gradual Capacity Reduction:** New mixed-pattern combinations will stop immediately.
- **UE Reconfiguration:** UEs holding mixed combinations are reverted to same-pattern subsets at their next reconfiguration.
- **SCell Contribution Loss:** Users on the mixed carrier will lose that SCell's contribution. It is recommended to schedule deactivation outside the busy hour on loaded sites.
- **PCell PUCCH Utilization:** PCell PUCCH utilization will fall back to the baseline within an hour as the codebook load reduces.

The deactivation process consists of disabling the function, deactivating the feature control, and verifying the state.

## Deactivation Steps

### Step 1: Disable the Function
Disable the mixed TDD CA function using the following command:

```bash
rancli set NodeRoot=1,NrFunction=1 mixedTddCaEnabled=false
```

### Step 2: Deactivate the Feature
Deactivate the feature control using the following command:

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=MixedTddPatternCa featureState=DEACTIVATED
```

### Step 3: Verify Deactivation
Verify that the feature state is set to `DEACTIVATED` using the following command:

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=MixedTddPatternCa featureState
```

# Cross-References

- [Activation Procedure](activation-procedure.md)
- [Parameters](parameters.md)
