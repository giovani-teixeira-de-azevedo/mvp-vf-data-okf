---
type: procedure
resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf#activation-procedure
title: Activation Procedure
description: Detailed step-by-step procedure and commands to activate and verify the
  Automated Energy Saver feature.
tags:
- activation
- procedure
- cli
- automated-energy-saver
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:38:10+00:00'
  source_sha256: 5702cb5858a6ea33
sources:
- resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf
  title: Automated Energy Saver
---

This section details the activation procedure for the Automated Energy Saver feature. It outlines the step-by-step CLI commands required to enable and configure the feature, as well as the recommended transition sequence from monitoring to autonomous execution.

## Preconditions and Impact

* **Traffic Impact:** None. Activation only starts the prediction model. When configured in `MONITOR` mode, the system produces decisions without executing them. Actual executed actions only begin after the operator transitions the node mode to `AUTO`, and every executed action is subject to the subordinate features' own safe transition mechanisms. Consequently, this procedure can be run safely during business hours.
* **License Requirement:** License key `FAK-31510` must be installed.
* **Feature Dependencies:** At least one subordinate energy feature must be licensed and activated.
* **PM History Requirement:** A minimum of 7 days of Performance Management (PM) history must be accumulated on the node. If this history is not available, the feature will run in `fallbackMode` until the necessary history accumulates.

## Recommended Rollout Timeline

To ensure network stability and verify prediction performance, the following rollout sequence is recommended:
1. **Week 1 (Monitor Phase):** Activate the feature in `MONITOR` mode.
2. **Review:** After one week, review the *Prediction Accuracy* KPI.
3. **Week 2–3 (Balanced Auto Phase):** If monitoring performance is satisfactory, switch to `AUTO` mode with `savingsLevel=BALANCED`.
4. **Aggressive Phase:** Only transition to `AGGRESSIVE` mode after two clean weeks of operation under the `BALANCED` policy.

## Step-by-Step Activation

### Step 1: Verify the License Key Installation
Ensure that the appropriate license key is enabled.
```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=AutomatedEnergySaver licenseState
```
*Expected Output:* `licenseState=ENABLED` (corresponding to key `FAK-31510`).

### Step 2: Activate the Feature Control
Enable the main feature state on the node control object.
```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=AutomatedEnergySaver featureState=ACTIVATED
```

### Step 3: Start in Monitor Mode with Intended Policy
Initialize the node in `MONITOR` mode with your planned target policy. Configuring the intended policy at this stage ensures that dry-run statistics accurately reflect the final desired configuration.
```bash
rancli set NodeRoot=1,NrFunction=1 energySaverMode=MONITOR savingsLevel=BALANCED
rancli set NodeRoot=1,NrFunction=1 protectedStart=06:30 protectedStop=22:00 wakeupLeadTime=30
```

### Step 4: Promote to Auto Mode
After one week of verifying that the dry-run KPIs are clean and stable, promote the mode to automatic execution:
```bash
rancli set NodeRoot=1,NrFunction=1 energySaverMode=AUTO
```

### Step 5: Verify Active Mode
Verify that the operational mode has transitioned successfully:
```bash
rancli get NodeRoot=1,NrFunction=1 energySaverMode
```

## Post-Change Verification

To confirm successful feature operation after promoting to `AUTO`:
* Verify that the counter `ctrAutoSleepActions` increments as expected during the next off-peak period.
* Confirm that the reactive wakeup counter `ctrReactiveWakeups` remains near zero, indicating stable prediction behavior.

# Cross-References

* [Parameters](parameters.md) — For details on `savingsLevel`, `energySaverMode`, and protected window configurations.
* [Performance Management](performance-management.md) — For details on the counters `ctrAutoSleepActions`, `ctrReactiveWakeups`, and the Prediction Accuracy KPI.
* [Deactivation Procedure](deactivation-procedure.md) — For instructions on how to disable the feature if necessary.
