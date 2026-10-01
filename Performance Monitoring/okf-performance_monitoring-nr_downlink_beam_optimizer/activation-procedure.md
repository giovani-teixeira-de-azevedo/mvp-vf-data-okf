---
type: procedure
resource: data/vodafone-mvp/raw/NR Downlink Beam Optimizer.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Step-by-step procedure to activate and configure the NR Downlink Beam
  Optimizer feature.
tags:
- activation
- configuration
- open-loop
- cli
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T17:02:12+00:00'
  source_sha256: a66a563c0d3824f1
sources:
- title: NR Downlink Beam Optimizer
  resource: data/vodafone-mvp/raw/NR Downlink Beam Optimizer.pdf
---

This section describes the step-by-step activation procedure for the NR Downlink Beam Optimizer feature, including traffic impact, preconditions, recommended rollout strategy, and CLI commands.

## Traffic Impact & Preconditions

### Traffic Impact
* **At Activation:** None.
* **During Operation:** Each applied grid change causes a sub-second SSB discontinuity inside the configured night window.
* **Open-Loop Mode:** In `OPEN_LOOP` mode, there is no autonomous change at all, making the activation itself traffic-safe at any hour.

For more details on network behavior, see [Network Impact](network-impact.md).

### Preconditions
Before starting the activation procedure, ensure the following preconditions are met:
* License key `FAK-30170` is installed.
* Massive MIMO baseline feature is active.
* NR Flexible Cell Shaping variants are deactivated on target cells.

For more details on feature requirements, see [Feature Dependencies](feature-depedencies.md).

---

## Recommended Rollout Strategy
1. Activate the feature in `OPEN_LOOP` mode on a cluster for two weeks.
2. Review the recommendation records against local knowledge.
3. Apply one or two recommendations manually.
4. Switch validated cells to `CLOSED_LOOP` mode only after successful manual validation.

---

## Step-by-Step Activation Procedure

The activation process consists of verifying the license, activating the feature, configuring and enabling `OPEN_LOOP` per cell, and verifying the configuration.

### Step 1: Verify the License Key is Installed
Execute the following command to verify that the license key is installed and enabled:

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=DlBeamOptimizer licenseState
```

**Expected Output:**
```text
licenseState=ENABLED (key FAK-30170)
```

### Step 2: Activate the Feature
Activate the feature at the function level:

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=DlBeamOptimizer featureState=ACTIVATED
```

### Step 3: Enable Open-Loop Optimization on Cell N1A
Configure the evaluation period, change hysteresis, and enable open-loop mode on the target cell (e.g., `N1A`):

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A,BeamOptimizer=1 evalPeriod=24 changeHysteresis=1.0
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A,BeamOptimizer=1 optimizerMode=OPEN_LOOP
```

*Note: For parameter details, see [Parameters](parameters.md).*

### Step 4: Verify Configuration
Verify that the optimizer mode is set correctly and check the recommended grid:

```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A,BeamOptimizer=1 optimizerMode recommendedGrid
```

### Post-Activation Verification
After the configured `evalPeriod` elapses:
1. Read the `recommendedGrid` parameter.
2. Check that the performance counter `ctrEvalSkippedLowSamples` is not incrementing.

For more details on performance monitoring, see [Performance Management](performance-management.md).

---

# Cross-References
* [Feature Dependencies](feature-depedencies.md)
* [Feature Operation](feature-operation.md)
* [Network Impact](network-impact.md)
* [Parameters](parameters.md)
* [Performance Management](performance-management.md)
* [Deactivation Procedure](deactivation-procedure.md)
