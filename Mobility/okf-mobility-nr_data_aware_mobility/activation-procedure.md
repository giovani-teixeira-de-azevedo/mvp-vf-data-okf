---
type: procedure
resource: data/vodafone-mvp/raw/NR Data-Aware Mobility.pdf#activation-procedure
title: Activation Procedure
description: Step-by-step procedure to activate and configure the NR Data-Aware Mobility
  feature.
tags:
- activation
- configuration
- rancli
- nr-data-aware-mobility
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T09:07:05+00:00'
  source_sha256: d23f4446cd0f61f2
sources:
- title: NR Data-Aware Mobility
  resource: data/vodafone-mvp/raw/NR Data-Aware Mobility.pdf
---

This section outlines the step-by-step procedure to activate and configure the NR Data-Aware Mobility feature using the RAN CLI.

## Traffic Impact & Rollout Strategy

* **Traffic Impact:** None. Activation changes only the timing and target choice of future handovers; no reconfiguration touches connected UEs at activation time.
* **Scheduling:** Business-hours activation is safe and recommended, as the feature requires active data traffic to demonstrate and verify its behavior.
* **Rollout Strategy:** 
  1. Start with `dataAwareMobMode=TIMING_ONLY` on a pilot cluster.
  2. Verify that the Escalation Ratio and Deferred HO Failure KPIs stay within acceptable bounds for one week.
  3. Enable `TIMING_AND_SCORING` and widen the rollout across the network.

## Preconditions

Before proceeding with the activation, ensure the following conditions are met:
* License key **FAK-33140** is installed.
* NR Mobility is operating with healthy baseline KPIs. Do not deploy deferral on top of an already failing mobility configuration.
* Xn Resource Status Reporting is confirmed toward key neighbor cells if `TIMING_AND_SCORING` is intended.

---

## Step-by-Step Activation

### Step 1: Verify the License Key Installation
Query the license state to ensure the required license key is active.

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=NrDataAwareMobility licenseState
```
* **Expected Output:** `licenseState=ENABLED` (key FAK-33140)

### Step 2: Activate the Feature Control
Activate the feature state at the network function level.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=NrDataAwareMobility featureState=ACTIVATED
```

### Step 3: Enable and Tune Per Cell
Configure the feature mode and tuning parameters on the target cell (e.g., `N1A`).

```bash
# Set the operational mode
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A dataAwareMobMode=TIMING_ONLY

# Configure deferral offsets, time-to-trigger, and search window
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A deferOffset=2 deferTtt=256 gapSearchWindow=500

# Set the critical RSRP floor
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A criticalRsrpFloor=-116
```

### Step 4: Verify Cell Configuration
Confirm that the operational mode has been successfully applied to the cell.

```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A dataAwareMobMode
```

---

## Post-Change Verification

After completing the activation steps, perform the following verification tasks:
1. **Real-time Verification:** Confirm that the performance counters `ctrHoDeferred` and `ctrHoGapExecuted` increment during periods of busy traffic.
2. **KPI Review:** Review the Deferred HO Failure KPI after 24 hours of operation to ensure stability.

# Cross-References

* For details on the parameters configured in Step 3, see [Parameters](parameters.md).
* For information on performance counters and KPIs, see [Performance Management](performance-management.md).
* To revert these changes, see [Deactivation Procedure](deactivation-procedure.md).
* For license and feature dependencies, see [Feature Dependencies](feature-depedencies.md).
