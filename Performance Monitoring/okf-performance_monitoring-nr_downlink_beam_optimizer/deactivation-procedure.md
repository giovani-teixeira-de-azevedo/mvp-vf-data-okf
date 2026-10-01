---
type: procedure
resource: data/vodafone-mvp/raw/NR Downlink Beam Optimizer.pdf#deactivation-procedure
title: DEACTIVATION PROCEDURE
description: Step-by-step procedure to disable and deactivate the NR Downlink Beam
  Optimizer feature and optionally restore the default grid.
tags:
- deactivation
- beam-optimizer
- rancli
- configuration
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T14:13:08+00:00'
  source_sha256: 88598fca7867db01
sources:
- resource: data/vodafone-mvp/raw/NR Downlink Beam Optimizer.pdf
  title: NR Downlink Beam Optimizer
---

The deactivation procedure for the NR Downlink Beam Optimizer feature disables optimization per cell, optionally restores the default radio grid, and deactivates the feature control.

## Traffic Impact and Grid Behavior

* **Traffic Impact:** None, unless a grid restore is requested.
* **Grid Behavior:** Deactivation freezes the currently active grid; it does not automatically revert to the default grid.
* **Default Grid Reversion:** If reversion to the default grid is desired, it is applied as a normal grid change (resulting in a sub-second SSB gap) and should be performed during the night window.
* **Documentation:** Record the final grid version in the site documentation so future audits can explain the non-default beam configuration.

## Step-by-Step Procedure

### Step 1: Disable optimization per cell
This step disables the optimizer per cell while retaining the currently active grid.

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A,BeamOptimizer=1 optimizerMode=DISABLED
```

### Step 2: Restore the default grid (Optional)
This step restores the radio default grid. It is recommended to perform this action inside the change window (night window recommended).

```bash
rancli action NodeRoot=1,NrFunction=1,NrCell=N1A,BeamOptimizer=1 restoreDefaultGrid
```

### Step 3: Deactivate the feature
Deactivate the feature control.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=DlBeamOptimizer featureState=DEACTIVATED
```

### Step 4: Verify
Verify that the feature state has been successfully deactivated.

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=DlBeamOptimizer featureState
```

# Cross-References

* [Activation Procedure](activation-procedure.md)
* [Parameters](parameters.md)
* [Feature Overview](feature-overview.md)
