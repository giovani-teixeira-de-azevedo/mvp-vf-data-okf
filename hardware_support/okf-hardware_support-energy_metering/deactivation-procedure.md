---
type: procedure
resource: data/vodafone-mvp/raw/Energy Metering.pdf#deactivation-procedure
title: Deactivation Procedure
description: Step-by-step instructions to deactivate the Energy Metering feature and
  verify the deactivation.
tags:
- RAN
- O&M
- Energy Metering
- Deactivation
- CLI
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:43:19+00:00'
  source_sha256: 7eb0b78ed5474cc4
sources:
- title: Energy Metering
  resource: data/vodafone-mvp/raw/Energy Metering.pdf
---

This section describes the procedure to deactivate the Energy Metering feature and verify its status. Deactivating the feature stops energy accumulation and counter reporting but has no impact on cell traffic or radio behavior.

## Impact & Pre-requisites

- **Traffic Impact:** None. Deactivation does not change radio behavior.
- **Counter Behavior:** Lifetime cumulative energy attributes retain their last recorded values.
- **Pre-deactivation Check:** Confirm that no active reporting pipeline (such as sustainability dashboards or savings verification for energy feature trials) is dependent on this node's counters. Gaps in the energy time-series are awkward to interpolate.

Step 1 deactivates the feature control, and Step 2 verifies that the configuration is retained for future re-activation while momentary attributes are reported as unavailable.

---

## 1. Deactivate the Feature

Execute the following CLI command to set the feature state to deactivated:

```bash
rancli set NodeRoot=1,EquipmentFunction=1,FeatureCtrl=EnergyMetering featureState=DEACTIVATED
```

## 2. Verify Deactivation

To verify that the feature is deactivated and that the momentary attributes report as unavailable, query the feature state and the radio unit's momentary power:

```bash
rancli get NodeRoot=1,EquipmentFunction=1,FeatureCtrl=EnergyMetering featureState
rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1 momentaryPower
```

# Cross-References

* [Activation Procedure](activation-procedure.md)
