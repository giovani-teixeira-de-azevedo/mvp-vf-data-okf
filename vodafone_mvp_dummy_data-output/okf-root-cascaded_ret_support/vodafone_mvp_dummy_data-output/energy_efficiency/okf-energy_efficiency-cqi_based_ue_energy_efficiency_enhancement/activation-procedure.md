---
type: procedure
resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Step-by-step activation procedure and CLI commands for CQI-Based UE Energy
  Efficiency Enhancement.
tags:
- activation
- rancli
- cqiEnergyEffMode
- energy-efficiency
- procedure
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T14:17:42+00:00'
  source_sha256: f5d38301ffed9d4a
sources:
- title: CQI-Based UE Energy Efficiency Enhancement
  resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf
---

This section provides the step-by-step activation procedure for the CQI-Based UE Energy Efficiency Enhancement feature, including prerequisites, recommended rollout strategy, `rancli` commands, and post-activation verification.

## Prerequisites and Traffic Impact

- **Traffic Impact:** None. The feature changes scheduling preferences and CSI configurations incrementally as UEs are classified. No cell restart or lock is required, and existing connections are reconfigured only through standard RRC procedures. The procedure can be executed during business hours.
- **Preconditions:**
  - License key `FAK-31525` installed.
  - Connected Mode DRX activated on target cells.

## Recommended Rollout Order

1. Start with `cqiEnergyEffMode=RACE_TO_SLEEP_ONLY` on a pilot cluster. This mode involves no RRC reconfiguration and no MCS bias, making it the lowest-risk step.
2. Observe the Active Time Efficiency KPI for one week.
3. Move to `FULL` mode.

## Step-by-Step Activation Procedure

### Step 1: Verify the License Key
Verify that the license key is installed:

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=CqiUeEnergyEff licenseState
```

**Expected output:** `licenseState=ENABLED` (key FAK-31525).

### Step 2: Activate the Feature
Activate the feature control:

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=CqiUeEnergyEff featureState=ACTIVATED
```

### Step 3: Pilot Mode Configuration
Enable race-to-sleep pilot mode per cell:

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A cqiEnergyEffMode=RACE_TO_SLEEP_ONLY
```

### Step 4: Full Mode Promotion
Promote to FULL mode with explicit thresholds so the configuration is auditable:

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A cqiEnergyEffMode=FULL
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A highCqiThr=11 lowCqiThr=5 mcsBackoff=2
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A relaxedCsiPeriod=80
```

### Step 5: Verify Configuration
Verify the active mode:

```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A cqiEnergyEffMode
```

## Post-Change Verification

- Confirm `ctrRtsBursts` increments within the first busy hour.
- Confirm that downlink throughput KPIs remain at baseline.

# Cross-References

- [Parameters](parameters.md)
- [Performance Management](performance-management.md)
- [Deactivation Procedure](deactivation-procedure.md)
