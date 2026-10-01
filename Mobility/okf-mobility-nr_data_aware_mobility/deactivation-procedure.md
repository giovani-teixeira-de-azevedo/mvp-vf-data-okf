---
type: procedure
resource: data/vodafone-mvp/raw/NR Data-Aware Mobility.pdf#deactivation-procedure
title: DEACTIVATION PROCEDURE
description: Step-by-step procedure to deactivate the NR Data-Aware Mobility feature
  and restore baseline mobility behavior.
tags:
- deactivation
- rancli
- configuration
- nr-mobility
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T10:02:36+00:00'
  source_sha256: 1cc2a35ee9ec8a6c
sources:
- title: NR Data-Aware Mobility
  resource: data/vodafone-mvp/raw/NR Data-Aware Mobility.pdf
---

This section describes the step-by-step procedure to deactivate the NR Data-Aware Mobility feature and restore baseline NR Mobility behavior.

## Overview and Impact

* **Traffic Impact:** None.
* **Service Impact:** Deactivation reverts mobility to the baseline NR Mobility behavior for all future events. Any handover currently in a deferral window executes immediately under baseline rules.
* **Requirements:** No cell lock or maintenance window is required. The license key can remain installed for later re-activation.

## Deactivation Steps

### Step 1: Disable per Cell
Disable the function on a per-cell basis. Once disabled, deferral windows drain within at most 2 seconds.

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A dataAwareMobMode=DISABLED
```

### Step 2: Deactivate the Feature
Deactivate the feature control state.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=NrDataAwareMobility featureState=DEACTIVATED
```

### Step 3: Verify
Verify that the baseline mobility behavior is restored by checking the cell configuration.

```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A dataAwareMobMode
```

# Cross-References

* [Activation Procedure](activation-procedure.md) - For details on how to re-activate the feature.
* [Parameters](parameters.md) - For details on the `dataAwareMobMode` and `featureState` parameters.
