---
type: procedure
resource: data/vodafone-mvp/raw/Coverage-Optimized Uplink Transmission High-Band.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Step-by-step procedure to activate and configure the Coverage-Optimized
  Uplink Transmission High-Band feature.
tags:
- Testing
- activation
- configuration
- high-band
- uplink-coverage
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T11:22:21+00:00'
  source_sha256: 168be650d039d524
sources:
- title: Coverage-Optimized Uplink Transmission High-Band
  resource: data/vodafone-mvp/raw/Coverage-Optimized Uplink Transmission High-Band.pdf
---

This section describes the step-by-step procedure to activate and configure the Coverage-Optimized Uplink Transmission High-Band feature.

## Traffic Impact & Scheduling
* **Traffic Impact:** None. Activation only arms the adaptation logic; existing connections are untouched, and coverage mode engages per UE only when its uplink degrades.
* **Scheduling:** The procedure can be executed during business hours.

## Preconditions
* License key **FAK-33110** must be installed.
* **Physical Layer High-Band** and **Scheduler High-Band** must be operational on the target cells.
* If **Optimized Uplink Peak Throughput High-Band** is active, be aware that peak mode will be suppressed for edge UEs.

## Recommended Rollout Order
1. Activate on a cluster of border cells with the default thresholds and `covOptUlTxMode=ADAPTIVE`.
2. Observe the KPIs for one week.
3. Tune `covEnterThr` per cell based on the Coverage Mode Ratio before wider rollout.

---

## Step-by-Step Procedure

### Step 1: Verify the License Key is Installed
Verify the license before making any configuration changes.

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=CovOptUlTxHighBand licenseState
```

**Expected Output:**
```text
licenseState=ENABLED (key FAK-33110)
```

### Step 2: Activate the Feature
Activate the feature control on the node.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=CovOptUlTxHighBand featureState=ACTIVATED
```

### Step 3: Enable and Tune Per Cell
Enable the function per cell with explicit thresholds so the configuration is auditable.

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A covOptUlTxMode=ADAPTIVE
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A covEnterThr=0 covExitThr=5
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A maxPuschRep=4 maxPrbCovMode=8
```

### Step 4: Verify Cell-Level State
Verify the cell-level state.

```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A covOptUlTxMode
```

---

## Post-Activation Verification
After activation, confirm within 24 hours that:
* The counter `ctrCovModeEntries` increments on border cells.
* **Edge Report Success** exceeds its baseline.

# Cross-References
* [Feature Dependencies](feature-depedencies.md)
* [Parameters](parameters.md)
* [Performance Management](performance-management.md)
* [Deactivation Procedure](deactivation-procedure.md)
