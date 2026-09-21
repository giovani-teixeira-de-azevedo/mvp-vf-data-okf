---
type: procedure
resource: data/vodafone-mvp/raw/NR Massive MIMO Sleep Mode.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Step-by-step activation procedure for the Massive MIMO Sleep Mode feature,
  including license verification, cell-level configuration, and initial verification.
tags:
- massive-mimo
- sleep-mode
- activation
- rancli
- configuration
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:42:07+00:00'
  source_sha256: cbc496fb0074203c
sources:
- title: NR Massive MIMO Sleep Mode
  resource: data/vodafone-mvp/raw/NR Massive MIMO Sleep Mode.pdf
---

The **Activation Procedure** outlines the step-by-step process required to enable and configure the NR Massive MIMO Sleep Mode feature. Activating this feature has **no traffic impact** because activation only arms the sleep logic; no branch is gated at the time of activation, and sleep entry itself is only triggered when the cell is already under low-load conditions. Consequently, this procedure can safely be executed during business hours.

## Preconditions

Before commencing the activation procedure, ensure that the following requirements are met:
* **License Key**: The license key `FAK-31240` must be installed via the normal license management flow.
* **MIMO Baseline**: The target cells must be running a Massive MIMO baseline feature.
* **Feature Coexistence**: NR Flexible Cell Shaping High-Band must not be enabled on the same cells (refer to [Feature Dependencies](feature-depedencies.md)).

## Recommended Rollout Strategy

To ensure network stability and enable performance auditing, the following phased rollout order is recommended:
1. Activate the feature on a small cluster of cells with `sleepMode=PARTIAL_ONLY`.
2. Observe the KPIs (such as counters) for one to two weeks.
3. Widen the configuration to `PARTIAL_AND_DEEP` on those cells and expand the rollout to further cells.

---

## Step-by-Step Activation

### Step 1: Verify the License Key is Installed
Before modifying any configurations, confirm that the license key is enabled. Activating the feature without a valid key results in a `licenseState=DISABLED` state that can be easily overlooked.

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=MassiveMimoSleep licenseState
```

**Expected Output:**
```text
licenseState=ENABLED (key FAK-31240)
```

### Step 2: Activate the Feature Node-Wide
Arm the sleep logic globally at the node-wide function level.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=MassiveMimoSleep featureState=ACTIVATED
```

### Step 3: Enable and Tune Per Cell
Enable the feature and configure thresholds for each target cell. The values below represent the default thresholds and are listed explicitly here to make the intended configuration fully auditable.

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A sleepMode=PARTIAL_AND_DEEP
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A prbLoadEnterThr=10 prbLoadExitThr=25
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A sleepEnterTimer=300
```

*Note: For parameter details and valid value ranges, see [Parameters](parameters.md).*

### Step 4: Verify Cell-Level Configuration
Confirm that the sleep mode has been successfully applied to the cell.

```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A sleepMode
```

---

## Post-Activation Verification

Once activation and configuration are complete, monitor the cell performance. During the next low-traffic period, confirm that the performance counter `ctrSleepEntries` begins incrementing, which verifies active entry into sleep mode. Refer to [Performance Management](performance-management.md) for details on sleep mode counters.

# Cross-References

* [Feature Dependencies](feature-depedencies.md) — For coexistence constraints and prerequisites.
* [Parameters](parameters.md) — Descriptions and defaults for cell-level settings like `sleepMode` and thresholds.
* [Performance Management](performance-management.md) — For details on tracking the `ctrSleepEntries` counter.
* [Deactivation Procedure](deactivation-procedure.md) — Steps to roll back or disable the feature if needed.
