---
type: procedure
resource: data/vodafone-mvp/raw/Extended Propagation Delay Support High-Band.pdf#deactivation-procedure
title: DEACTIVATION PROCEDURE
description: Detailed steps to disable and deactivate the Extended Propagation Delay
  Support High-Band feature, including rancli commands.
tags:
- deactivation
- rancli
- configuration
- FR2
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:26:57+00:00'
  source_sha256: 77bb7db24e5b734e
sources:
- title: Extended Propagation Delay Support High-Band
  resource: data/vodafone-mvp/raw/Extended Propagation Delay Support High-Band.pdf
---

This document describes the deactivation procedure for the Extended Propagation Delay Support High-Band feature. Deactivating this feature reverts the standard PRACH configuration, which triggers a brief cell restart and permanently removes coverage for User Equipments (UEs) located beyond the legacy cell range.

## Impact & Pre-requisites

*   **Traffic Impact:** Brief cell restart (30–60 seconds). Distant UEs situated beyond the legacy range will lose service permanently.
*   **Pre-requisite Validation:** Verify via the Timing Advance (TA) histogram counters that no active subscriber base depends on the extended range before initiating deactivation.
*   **Neighbor Cell Effects:** Expect the counter `ctrUeBeyondMaxRange` to rise on neighboring cells as displaced distant UEs attempt to access them.

The procedure must be executed in a maintenance window. It consists of three primary steps: locking the cell and reverting the per-carrier configuration, deactivating the feature-level switch, and verifying the final state.

---

## Step-by-Step Deactivation Procedure

### Step 1: Revert configuration per cell (maintenance window)

Lock the cell, revert the `extendedRangeMode` parameter to `DISABLED` on the sector carrier, and then unlock the cell.

```bash
rancli lock NodeRoot=1,NrFunction=1,NrCell=N1A 
rancli set NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 extendedRangeMode=DISABLED 
rancli unlock NodeRoot=1,NrFunction=1,NrCell=N1A 
```

### Step 2: Deactivate the feature

Deactivate the feature control after all carriers have been successfully reverted in Step 1.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=ExtPropDelayHighBand featureState=DEACTIVATED 
```

### Step 3: Verify configuration

Verify that the `extendedRangeMode` on the sector carrier is correctly set.

```bash
rancli get NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 extendedRangeMode
```

---

## Cross-References

*   [Activation Procedure](activation-procedure.md) - For enabling the feature.
*   [Parameters](parameters.md) - Details on `extendedRangeMode` and configuration options.
*   [Performance Management](performance-management.md) - Details on PM counters (such as `ctrUeBeyondMaxRange` or TA histogram counters) to monitor traffic before/after deactivation.
*   [Network Impact](network-impact.md) - Understanding cell restart behavior and KPI impacts.
