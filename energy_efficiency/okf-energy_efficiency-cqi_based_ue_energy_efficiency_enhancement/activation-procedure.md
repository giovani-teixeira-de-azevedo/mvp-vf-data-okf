---
type: procedure
resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Detailed step-by-step activation procedure for the CQI-Based UE Energy
  Efficiency Enhancement feature, including pilot rollout and full mode configuration.
tags:
- activation
- procedure
- rancli
- configuration
- race-to-sleep
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:38:41+00:00'
  source_sha256: f5d38301ffed9d4a
sources:
- resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf
  title: CQI-Based UE Energy Efficiency Enhancement
---

This document outlines the step-by-step activation procedure for the CQI-Based UE Energy Efficiency Enhancement feature. It details the traffic impact, preconditions, recommended rollout strategy, and exact command-line interface execution steps.

## Preconditions & Traffic Impact

*   **Traffic Impact:** None. The feature changes scheduling preferences and CSI configurations incrementally as UEs are classified. No cell restart or lock is required, and existing connections are reconfigured only through standard RRC procedures. The procedure can be safely executed during business hours.
*   **Preconditions:**
    *   License key `FAK-31525` must be installed.
    *   Connected Mode DRX must be activated on the target cells.

## Recommended Rollout Strategy

A phased rollout is recommended to minimize network risk:
1.  **Pilot Cluster Phase:**
    *   Start by setting `cqiEnergyEffMode=RACE_TO_SLEEP_ONLY` on a pilot cluster. This mode involves no RRC reconfiguration and no MCS bias, representing the lowest-risk operational change.
    *   Observe the **Active Time Efficiency KPI** for one week.
2.  **Full Activation Phase:**
    *   Promote the mode to `FULL` after verifying pilot performance.

---

## Step-by-Step Activation

### Step 1: Verify the License Key Installation
Ensure the license key `FAK-31525` is installed and enabled on the target node.

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=CqiUeEnergyEff licenseState
```

**Expected output:**
```text
licenseState=ENABLED (key FAK-31525)
```

### Step 2: Activate the Feature Control
Enable the main feature control state.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=CqiUeEnergyEff featureState=ACTIVATED
```

### Step 3: Pilot Mode Activation (Race-to-Sleep Only)
Configure the target cell (e.g., `N1A`) to run in the conservative pilot mode.

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A cqiEnergyEffMode=RACE_TO_SLEEP_ONLY
```

### Step 4: Promote to Full Mode with Explicit Thresholds
To run the full feature with explicit, auditable thresholds, apply the following configuration parameters:

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A cqiEnergyEffMode=FULL
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A highCqiThr=11 lowCqiThr=5 mcsBackoff=2
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A relaxedCsiPeriod=80
```

### Step 5: Verify Cell Mode State
Confirm the operational mode of the cell is set as expected.

```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A cqiEnergyEffMode
```

---

## Post-Change Verification

After executing the configuration:
1.  Confirm that the counter `ctrRtsBursts` increments within the first busy hour (indicating active "Race-to-Sleep" operation).
2.  Verify that downlink throughput KPIs remain stable compared to the baseline.

# Cross-References

*   For details on the parameters configured in Step 4, see [Parameters](parameters.md).
*   For details on monitoring performance KPIs and counters like `ctrRtsBursts`, see [Performance Management](performance-management.md).
*   To reverse these changes, see [Deactivation Procedure](deactivation-procedure.md).
