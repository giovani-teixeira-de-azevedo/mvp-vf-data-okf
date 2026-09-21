---
type: procedure
resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf#deactivation-procedure
title: Deactivation Procedure
description: Step-by-step procedure to disable and deactivate the Dynamic Component
  Carrier Management feature.
tags:
- ran
- deactivation
- cli
- configuration
- dccm
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:38:00+00:00'
  source_sha256: 8e574dc4c067b1c3
sources:
- resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf
  title: Dynamic Component Carrier Management
---

This document outlines the step-by-step procedure to disable and deactivate the Dynamic Component Carrier Management (DCCM) feature. It details the traffic impact, behavioral changes, and the exact commands required to complete the process.

## Impact of Deactivation

- **Traffic Impact:** None.
- **SCell Selection:** Reverts to the static priority order for new setups.
- **Existing SCells:** Existing Secondary Cells (SCells) remain where they are and drift back to the static distribution as connections turn over. No forced reconfigurations are issued.
- **Load Spread:** Expect the per-carrier load spread to gradually widen back toward the pre-activation pattern. If the deactivation is part of an A/B evaluation, retain the Performance Management (PM) data.

## Deactivation Steps

The deactivation process consists of disabling the manager (halting scoring and rebalancing immediately), deactivating the feature control, and verifying the state.

### Step 1: Disable the Function
This disables the manager, immediately halting scoring and rebalancing.

```bash
rancli set NodeRoot=1,NrFunction=1 dynamicCcMgmtEnabled=false
```

### Step 2: Deactivate the Feature Control
This deactivates the feature control state.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=DynamicCcMgmt featureState=DEACTIVATED
```

### Step 3: Verify
Verify that the feature state is set correctly.

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=DynamicCcMgmt featureState
```

# Cross-References

- To activate the feature, see the [Activation Procedure](activation-procedure.md).
- To view associated configuration settings, see [Parameters](parameters.md).
- For details on performance implications, see [Network Impact](network-impact.md).
