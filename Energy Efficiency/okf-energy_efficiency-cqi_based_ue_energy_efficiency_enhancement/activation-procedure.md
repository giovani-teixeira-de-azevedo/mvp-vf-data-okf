---
type: procedure
resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Activation procedure, preconditions, rollout strategy, CLI commands,
  and post-change verification for CQI-based UE energy efficiency enhancement.
tags:
- activation
- procedure
- rancli
- energy-efficiency
- cqi
- rollout
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-25T17:21:29+00:00'
  source_sha256: f5d38301ffed9d4a
sources:
- resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf
  title: CQI-Based UE Energy Efﬁciency Enhancement
---

Traffic impact: none. The feature changes scheduling preferences and CSI configurations incrementally as UEs are classified; no cell restart or lock is required, and existing connections are reconfigured only through standard RRC procedures. The procedure can be executed during business hours.

Preconditions: license key FAK-31525 installed; Connected Mode DRX activated on the target cells.

Recommended rollout order: start with `cqiEnergyEffMode=RACE_TO_SLEEP_ONLY` on a pilot cluster (this mode involves no RRC reconfiguration and no MCS bias, so it is the lowest-risk step), observe the Active Time Efficiency KPI for one week, then move to FULL.

Step 1 verifies the license; step 2 activates the feature control; step 3 enables the pilot mode per cell; step 4 promotes to FULL with explicit thresholds so the configuration is auditable; step 5 verifies. Post-change, confirm `ctrRtsBursts` increments within the first busy hour and that downlink throughput KPIs remain at baseline.

## Procedure Steps

### 1. Verify the license key is installed

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=CqiUeEnergyEff licenseState
```
Expected: `licenseState=ENABLED (key FAK-31525)`

### 2. Activate the feature

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=CqiUeEnergyEff featureState=ACTIVATED
```

### 3. Pilot: race-to-sleep only

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A cqiEnergyEffMode=RACE_TO_SLEEP_ONLY
```

### 4. Full mode with explicit thresholds

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A cqiEnergyEffMode=FULL
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A highCqiThr=11 lowCqiThr=5 mcsBackoff=2
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A relaxedCsiPeriod=80
```

### 5. Verify

```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A cqiEnergyEffMode
```

# Cross-References

- [FEATURE DEPEDENCIES](feature-depedencies.md)
- [PARAMETERS](parameters.md)
- [PERFORMANCE MANAGEMENT](performance-management.md)
- [DEACTIVATION PROCEDURE](deactivation-procedure.md)
