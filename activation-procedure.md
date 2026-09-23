---
type: procedure
resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Step-by-step activation procedure, preconditions, and verification steps
  for the Automated Energy Saver feature.
tags:
- activation
- procedure
- automated-energy-saver
- rancli
- configuration
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T18:18:00+00:00'
  source_sha256: 5702cb5858a6ea33
sources:
- resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf
  title: Automated Energy Saver
---

This section details the step-by-step procedure for activating the Automated Energy Saver feature, including traffic impact considerations, preconditions, recommended rollout timeline, configuration commands, and post-activation verification.

## Impact and Preconditions

### Traffic Impact
Traffic impact is none. Activation only starts the prediction model and, in MONITOR mode, produces decisions without executing them. Executed actions begin only after the operator moves the node to AUTO, and every executed action goes through the subordinate features' own safe transition mechanisms. The procedure can be run during business hours.

### Preconditions
- License key **FAK-31510** installed.
- At least one subordinate energy feature licensed and activated.
- At least 7 days of PM history on the node (otherwise the feature runs in `fallbackMode` until history accumulates).

### Recommended Rollout Order
1. Activate in `MONITOR` mode for one week.
2. Review the Prediction Accuracy KPI.
3. Switch to `AUTO` with `savingsLevel=BALANCED`.
4. Consider `AGGRESSIVE` only after two clean weeks.

---

## Step-by-Step Activation Procedure

### Step 1: Verify the license key is installed
```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=AutomatedEnergySaver licenseState
```
**Expected:** `licenseState=ENABLED` (key FAK-31510)

### Step 2: Activate the feature
```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=AutomatedEnergySaver featureState=ACTIVATED
```

### Step 3: Start in MONITOR mode with intended policy
Start the node in MONITOR mode with the intended policy so that dry-run statistics reflect the final configuration:
```bash
rancli set NodeRoot=1,NrFunction=1 energySaverMode=MONITOR savingsLevel=BALANCED
rancli set NodeRoot=1,NrFunction=1 protectedStart=06:30 protectedStop=22:00 wakeupLeadTime=30
```

### Step 4: Promote to AUTO
After one week of clean MONITOR KPIs, promote the mode to AUTO:
```bash
rancli set NodeRoot=1,NrFunction=1 energySaverMode=AUTO
```

### Step 5: Verify
Verify the engaged state:
```bash
rancli get NodeRoot=1,NrFunction=1 energySaverMode
```

---

## Post-Change Verification

Post-change, confirm that:
- `ctrAutoSleepActions` increments during the next off-peak period.
- `ctrReactiveWakeups` stays near zero.

# Cross-References

- [PARAMETERS](parameters.md)
- [PERFORMANCE MANAGEMENT](performance-management.md)
- [DEACTIVATION PROCEDURE](deactivation-procedure.md)
