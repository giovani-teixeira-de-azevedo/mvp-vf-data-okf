---
type: procedure
resource: data/vodafone-mvp/raw/NR Downlink Beam Optimizer.pdf#deactivation-procedure
title: DEACTIVATION PROCEDURE
description: Step-by-step procedure to deactivate the NR Downlink Beam Optimizer and
  optionally restore the default grid.
tags:
- NR Downlink Beam Optimizer
- Deactivation
- CLI
- RAN Configuration
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T17:02:14+00:00'
  source_sha256: 88598fca7867db01
sources:
- title: NR Downlink Beam Optimizer
  resource: data/vodafone-mvp/raw/NR Downlink Beam Optimizer.pdf
---

This section describes the step-by-step procedure to deactivate the NR Downlink Beam Optimizer feature and optionally restore the default radio grid.

## Traffic Impact and Considerations

* **Traffic Impact:** None, unless a grid restore is requested.
* **Grid State:** Deactivation freezes the currently active grid; it does not automatically revert to the default grid.
* **Default Grid Reversion:** If reversion to the default grid is desired, it is applied as a normal grid change (causing a sub-second SSB gap) and should be performed during the night maintenance window.
* **Documentation:** Record the final grid version in the site documentation so that future audits can explain any non-default beam configuration.

## Step-by-Step Procedure

### Step 1: Disable Optimization per Cell
This step disables the optimizer per cell while retaining the currently active grid.

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A,BeamOptimizer=1 optimizerMode=DISABLED
```

### Step 2: Restore the Default Grid (Optional)
This step restores the radio default grid. It is recommended to perform this action inside the change window (night window).

```bash
rancli action NodeRoot=1,NrFunction=1,NrCell=N1A,BeamOptimizer=1 restoreDefaultGrid
```

### Step 3: Deactivate the Feature Control
Deactivate the feature control at the node level.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=DlBeamOptimizer featureState=DEACTIVATED
```

### Step 4: Verify Deactivation
Verify that the feature state has been successfully updated.

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=DlBeamOptimizer featureState
```

# Cross-References

* [Activation Procedure](activation-procedure.md)
* [Feature Operation](feature-operation.md)
