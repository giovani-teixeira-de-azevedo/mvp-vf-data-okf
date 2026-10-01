---
type: procedure
resource: data/vodafone-mvp/raw/NR Data-Aware Mobility.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Step-by-step procedure to activate and configure the NR Data-Aware Mobility
  feature.
tags:
- activation
- configuration
- rancli
- deployment
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T10:02:38+00:00'
  source_sha256: d23f4446cd0f61f2
sources:
- resource: data/vodafone-mvp/raw/NR Data-Aware Mobility.pdf
  title: NR Data-Aware Mobility
---

This section outlines the step-by-step activation procedure for the NR Data-Aware Mobility feature, including traffic impact, preconditions, CLI commands, and post-change verification.

## Traffic Impact

Activating the feature has **no traffic impact**. Activation only changes the timing and target choice of future handovers; no reconfiguration touches connected UEs at activation time. Business-hours activation is safe and in fact preferable, since the feature needs active data traffic to show its behavior.

## Preconditions

Before proceeding with the activation, ensure the following preconditions are met:
* **License Key:** License key `FAK-33140` must be installed (see [Feature Dependencies](feature-depedencies.md)).
* **Baseline KPIs:** NR Mobility must be operating with healthy baseline KPIs. Do not deploy deferral on top of an already failing mobility configuration.
* **Xn Reporting:** Xn Resource Status Reporting must be confirmed toward key neighbors if `TIMING_AND_SCORING` is intended.
* **Recommended Rollout Strategy:** 
  1. Start with `dataAwareMobMode=TIMING_ONLY` on a pilot cluster.
  2. Verify that the Escalation Ratio and Deferred HO Failure stay in bounds for one week.
  3. Enable `TIMING_AND_SCORING` and widen the rollout.

---

## Step-by-Step Activation

### Step 1: Verify the License Key Installation
Verify that the license key is installed and enabled.

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=NrDataAwareMobility licenseState
```
* **Expected Output:** `licenseState=ENABLED` (key `FAK-33140`)

### Step 2: Activate the Feature Control
Activate the feature at the network function level.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=NrDataAwareMobility featureState=ACTIVATED
```

### Step 3: Enable and Tune Per Cell
Enable the function per cell with explicit deferral bounds. The following example configures cell `N1A` with `TIMING_ONLY` mode and recommended initial parameters:

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A dataAwareMobMode=TIMING_ONLY
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A deferOffset=2 deferTtt=256 gapSearchWindow=500
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A criticalRsrpFloor=-116
```

### Step 4: Verify Cell Configuration
Verify that the cell-level mode has been successfully applied.

```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A dataAwareMobMode
```

---

## Post-Change Verification

After completing the activation steps, perform the following verification tasks:
1. Confirm that the performance counters `ctrHoDeferred` and `ctrHoGapExecuted` increment during busy traffic (see [Performance Management](performance-management.md)).
2. Review the Deferred HO Failure KPI after 24 hours to ensure stability.

# Cross-References

* [Feature Dependencies](feature-depedencies.md) — License key and functional prerequisites
* [Parameters](parameters.md) — Detailed description of `dataAwareMobMode`, `deferOffset`, `deferTtt`, `gapSearchWindow`, and `criticalRsrpFloor`
* [Performance Management](performance-management.md) — Performance counters and KPIs for post-change verification
* [Deactivation Procedure](deactivation-procedure.md) — Steps to revert the activation if necessary
