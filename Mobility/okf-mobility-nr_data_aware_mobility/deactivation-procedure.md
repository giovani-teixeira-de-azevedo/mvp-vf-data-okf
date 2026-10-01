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
- mobility
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T09:07:05+00:00'
  source_sha256: 1cc2a35ee9ec8a6c
sources:
- title: NR Data-Aware Mobility
  resource: data/vodafone-mvp/raw/NR Data-Aware Mobility.pdf
---

This section describes the procedure to deactivate the NR Data-Aware Mobility feature. Deactivation reverts mobility to the baseline NR Mobility behavior for all future events, and any handover currently in a deferral window executes immediately under baseline rules.

## Impact and Requirements

* **Traffic impact:** None.
* **Cell lock / Maintenance window:** Not required.
* **License key:** Can remain installed for later re-activation.

## Deactivation Steps

### Step 1: Disable per cell
Disable the function per cell. Deferral windows drain within at most 2 seconds.

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A dataAwareMobMode=DISABLED
```

### Step 2: Deactivate the feature
Deactivate the feature control.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=NrDataAwareMobility featureState=DEACTIVATED
```

### Step 3: Verify
Verify that baseline mobility behavior is restored.

```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A dataAwareMobMode
```

# Cross-References

* [Activation Procedure](activation-procedure.md) - For details on how to activate the feature.
* [Parameters](parameters.md) - For details on the `dataAwareMobMode` parameter and other configuration options.
