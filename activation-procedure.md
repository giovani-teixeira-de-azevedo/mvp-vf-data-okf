---
type: procedure
resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Activation procedure, preconditions, traffic impact, and step-by-step
  CLI commands for enabling Automated Energy Saver.
tags:
- automated-energy-saver
- activation
- rancli
- energy-saving
- configuration
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-24T08:40:48+00:00'
  source_sha256: 5702cb5858a6ea33
sources:
- resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf
  title: Automated Energy Saver
---

This section defines the activation procedure, preconditions, traffic impact considerations, and step-by-step CLI commands for enabling the Automated Energy Saver feature.

## Operational Considerations and Preconditions

### Traffic Impact
Traffic impact: none. Activation only starts the prediction model and, in MONITOR mode, produces decisions without executing them. Executed actions begin only after the operator moves the node to AUTO, and every executed action goes through the subordinate features' own safe transition mechanisms. The procedure can be run during business hours.

### Preconditions
- License key FAK-31510 installed.
- At least one subordinate energy feature licensed and activated.
- At least 7 days of PM history on the node (otherwise the feature runs in `fallbackMode` until history accumulates).

### Recommended Rollout Order
1. Activate in MONITOR mode for one week.
2. Review the Prediction Accuracy KPI.
3. Switch to AUTO with `savingsLevel=BALANCED`.
4. Only consider AGGRESSIVE after two clean weeks.

---

## Step-by-Step Activation Procedure

Step 1 verifies the license; step 2 activates the feature control; step 3 starts the node in MONITOR mode with the intended policy so that the dry-run statistics reflect the final configuration; step 4 promotes to AUTO after review; step 5 verifies the engaged state. Post-change, confirm that `ctrAutoSleepActions` increments during the next off-peak period and that `ctrReactiveWakeups` stays near zero.

### Step 1: Verify the license key is installed
```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=AutomatedEnergySaver licenseState
```
*Expected output:* `# Expected: licenseState=ENABLED (key FAK-31510)`

### Step 2: Activate the feature
```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=AutomatedEnergySaver featureState=ACTIVATED
```

### Step 3: Start in monitor mode with intended policy
```bash
rancli set NodeRoot=1,NrFunction=1 energySaverMode=MONITOR savingsLevel=BALANCED
rancli set NodeRoot=1,NrFunction=1 protectedStart=06:30 protectedStop=22:00 wakeupLeadTime=30
```

### Step 4: After one week of clean MONITOR KPIs, promote to AUTO
```bash
rancli set NodeRoot=1,NrFunction=1 energySaverMode=AUTO
```

### Step 5: Verify
```bash
rancli get NodeRoot=1,NrFunction=1 energySaverMode
```

---

## Post-Change Verification

Post-change, confirm that:
- `ctrAutoSleepActions` increments during the next off-peak period.
- `ctrReactiveWakeups` stays near zero.

# Cross-References

- [PARAMETERS](parameters.md) — Configuration parameters used during feature activation.
- [PERFORMANCE MANAGEMENT](performance-management.md) — Performance metrics and counters monitored during and after activation.
