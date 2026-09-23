---
type: procedure
resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Details the traffic impact, preconditions, rollout recommendations, and
  CLI commands for activating Dynamic Component Carrier Management.
tags:
- activation
- procedure
- dynamic-cc-management
- rancli
- configuration
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T15:05:04+00:00'
  source_sha256: 9c3401546223ae94
sources:
- title: Dynamic Component Carrier Management
  resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf
---

This section details the activation procedure for Dynamic Component Carrier Management, including traffic impact, preconditions, recommended rollout steps, post-activation verification, and CLI commands.

## Overview and Preconditions

- **Traffic impact:** None. New SCell setups immediately follow dynamic scoring; existing SCell configurations are only changed by paced rebalancing, so there is no signaling burst and no cell lock. Activation can proceed in business hours, though the first busy hour after activation should be observed.
- **Preconditions:**
  - License key `FAK-33013` installed
  - Baseline CA features active
  - Carrier relations defined for all co-sited carriers
  - PM baseline collected
- **Recommended rollout:** Enable with rebalancing effectively conservative (`rebalanceThr=90`) on a pilot site, verify Migration Productivity, then lower toward the default `80` and expand.
- **Post-activation:** Confirm `ctrScellSetups` increments and Load Spread trends down over the first days.

## Step-by-Step Activation

### Step 1: Verify the license key is installed
```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=DynamicCcMgmt licenseState
```
*Expected:* `licenseState=ENABLED` (key FAK-33013)

### Step 2: Activate the feature
```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=DynamicCcMgmt featureState=ACTIVATED
```

### Step 3: Enable and tune
```bash
rancli set NodeRoot=1,NrFunction=1 dynamicCcMgmtEnabled=true
rancli set NodeRoot=1,NrFunction=1 loadWeight=50 rsrpWeight=30 tputWeight=20
rancli set NodeRoot=1,NrFunction=1 rebalanceThr=80 maxMigrationsPerRop=100
```

### Step 4: Verify
```bash
rancli get NodeRoot=1,NrFunction=1 dynamicCcMgmtEnabled
```

# Cross-References

- [Parameters](parameters.md)
- [Performance Management](performance-management.md)
- [Deactivation Procedure](deactivation-procedure.md)
