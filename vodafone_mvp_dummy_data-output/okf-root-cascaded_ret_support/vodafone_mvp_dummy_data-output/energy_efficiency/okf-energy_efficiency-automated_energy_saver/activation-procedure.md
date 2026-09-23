---
type: procedure
resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Provides the activation procedure, preconditions, recommended rollout
  strategy, and rancli commands for Automated Energy Saver.
tags:
- activation
- automated-energy-saver
- rancli
- monitor-mode
- auto-mode
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T14:06:04+00:00'
  source_sha256: 5702cb5858a6ea33
sources:
- resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf
  title: Automated Energy Saver
---

The **Activation Procedure** outlines the step-by-step process, preconditions, rollout guidelines, and CLI commands required to enable and safely transition the Automated Energy Saver feature into operation.

## Traffic Impact and Scheduling

- **Traffic Impact:** None.
- **Execution Timing:** The procedure can be executed during business hours. Activation starts the prediction model and, in `MONITOR` mode, produces decisions without executing them. Executed actions begin only after moving the node to `AUTO`, with every executed action governed by subordinate features' safe transition mechanisms.

## Preconditions and Recommended Rollout

### Preconditions
- License key **FAK-31510** must be installed.
- At least one subordinate energy feature must be licensed and activated.
- At least 7 days of PM history on the node (otherwise the feature operates in `fallbackMode` until history accumulates).

### Recommended Rollout Strategy
1. Activate in `MONITOR` mode for one week.
2. Review the Prediction Accuracy KPI.
3. Switch to `AUTO` with `savingsLevel=BALANCED`.
4. Consider `AGGRESSIVE` mode only after two clean weeks.

## Step-by-Step Activation

### Step 1: Verify License Installation
Verify that license key FAK-31510 is enabled:
```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=AutomatedEnergySaver licenseState
# Expected: licenseState=ENABLED (key FAK-31510)
```

### Step 2: Activate Feature Control
Enable the feature control state:
```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=AutomatedEnergySaver featureState=ACTIVATED
```

### Step 3: Start in MONITOR Mode
Start the node in MONITOR mode with the intended policy parameters so dry-run statistics reflect the target configuration:
```bash
rancli set NodeRoot=1,NrFunction=1 energySaverMode=MONITOR savingsLevel=BALANCED
rancli set NodeRoot=1,NrFunction=1 protectedStart=06:30 protectedStop=22:00 wakeupLeadTime=30
```

### Step 4: Promote to AUTO Mode
After one week of clean MONITOR KPIs, promote the mode to AUTO:
```bash
rancli set NodeRoot=1,NrFunction=1 energySaverMode=AUTO
```

### Step 5: Verify Engaged State
Verify the operational mode:
```bash
rancli get NodeRoot=1,NrFunction=1 energySaverMode
```

## Post-Change Verification

Post-change, confirm the following during the next off-peak period:
- `ctrAutoSleepActions` increments during the off-peak period.
- `ctrReactiveWakeups` stays near zero.

# Cross-References

- [FEATURE DEPEDENCIES](feature-depedencies.md)
- [PARAMETERS](parameters.md)
- [PERFORMANCE MANAGEMENT](performance-management.md)
- [DEACTIVATION PROCEDURE](deactivation-procedure.md)
