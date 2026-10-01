---
type: procedure
resource: data/vodafone-mvp/raw/NR Downlink Beam Optimizer.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Step-by-step procedure to activate and verify the NR Downlink Beam Optimizer
  feature.
tags:
- activation
- configuration
- open-loop
- rancli
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T14:33:56+00:00'
  source_sha256: a66a563c0d3824f1
sources:
- title: NR Downlink Beam Optimizer
  resource: data/vodafone-mvp/raw/NR Downlink Beam Optimizer.pdf
---

This section describes the step-by-step activation procedure for the NR Downlink Beam Optimizer feature. It outlines the traffic impact, preconditions, recommended rollout strategy, and the specific CLI commands required to activate and verify the feature.

## Traffic Impact & Preconditions

* **Traffic Impact:** None at activation. Each applied grid change causes a sub-second SSB discontinuity inside the configured night window. In `OPEN_LOOP` mode, there is no autonomous change at all, making activation itself traffic-safe at any hour.
* **Preconditions:**
  * License key `FAK-30170` must be installed.
  * License key `TESTING_LICENCE` must be installed.
  * Massive MIMO baseline feature must be active.
  * NR Flexible Cell Shaping variants must be deactivated on target cells.
* **Recommended Rollout:** Activate in `OPEN_LOOP` on a cluster for two weeks, review the recommendation records against local knowledge, apply one or two manually, and only then switch validated cells to `CLOSED_LOOP`.

## Step-by-Step Activation

The activation process consists of four steps: verifying the license, activating the feature, configuring and enabling `OPEN_LOOP` per cell, and verifying the configuration. After the evaluation period (`evalPeriod`) elapses, read `recommendedGrid` and check that `ctrEvalSkippedLowSamples` is not incrementing.

### Step 1: Verify the license key is installed
Execute the following command to verify the license state:
```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=DlBeamOptimizer licenseState
```
**Expected Output:**
```text
licenseState=ENABLED (key FAK-30170)
```

### Step 2: Activate the feature
Execute the following command to activate the feature:
```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=DlBeamOptimizer featureState=ACTIVATED
```

### Step 3: Enable open-loop optimization on cell N1A
Execute the following commands to configure and enable `OPEN_LOOP` mode on the target cell (e.g., `N1A`):
```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A,BeamOptimizer=1 evalPeriod=24 changeHysteresis=1.0
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A,BeamOptimizer=1 optimizerMode=OPEN_LOOP
```

### Step 4: Verify configuration
Execute the following command to verify the optimizer mode and recommended grid:
```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A,BeamOptimizer=1 optimizerMode recommendedGrid
```

# Cross-References

* [Feature Dependencies](feature-depedencies.md) — For details on preconditions and license requirements.
* [Feature Operation](feature-operation.md) — For details on open-loop and closed-loop modes.
* [Network Impact](network-impact.md) — For details on SSB discontinuity and traffic impact.
* [Parameters](parameters.md) — For details on `evalPeriod`, `changeHysteresis`, and `optimizerMode`.
* [Performance Management](performance-management.md) — For details on `ctrEvalSkippedLowSamples` and performance counters.
* [Deactivation Procedure](deactivation-procedure.md) — For details on how to disable or deactivate the feature.
