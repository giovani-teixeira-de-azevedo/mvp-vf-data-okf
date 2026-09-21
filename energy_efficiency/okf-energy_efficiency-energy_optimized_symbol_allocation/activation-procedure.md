---
type: procedure
resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Step-by-step procedure to verify licensing, activate, configure, and
  verify the Energy-Optimized Symbol Allocation feature.
tags:
- activation
- configuration
- rancli
- commissioning
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:41:03+00:00'
  source_sha256: 7907cbf45f1f5268
sources:
- resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf
  title: Energy-Optimized Symbol Allocation
---

The **Activation Procedure** provides a step-by-step guide to enabling, tuning, and verifying the Energy-Optimized Symbol Allocation feature on the gNodeB. This process is non-disruptive and can be performed during active traffic hours.

### Traffic Impact & Execution
* **Traffic Impact:** None. Compaction decisions are made per scheduling assignment with an automatic full-slot fallback always available.
* **UE Signaling:** Every allocation is signaled via standard Downlink Control Information (DCI); connected UEs follow the scheduling without requiring reconfiguration.
* **Service Interruption:** No cell lock, unlock, or node restart is required. The procedure can safely be executed during business hours.

### Preconditions
Before initiating the activation procedure, ensure the following requirements are met:
* **License Key:** License key `FAK-31540` must be installed.
* **Feature Dependencies:** *NR Micro Sleep Tx* must be activated on the same cells (otherwise, energy savings from symbol allocation will be negligible).
* **Hardware Requirements:** Radio generation R2 or later is required to support symbol-level muting.
* **Rollout Recommendation:** It is recommended to deploy the feature first on a pilot cluster using default parameters, verify Compaction BLER parity for one week, and then extend the deployment network-wide.

---

### Step-by-Step Activation

#### Step 1: Verify the License Key Installation
Verify that the `EnergyOptSymbolAlloc` license key is installed and enabled on the node.
```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=EnergyOptSymbolAlloc licenseState
```
* **Expected Output:** `licenseState=ENABLED` (associated with key `FAK-31540`)

#### Step 2: Activate the Feature Control
Enable the feature state globally at the feature control level.
```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=EnergyOptSymbolAlloc featureState=ACTIVATED
```

#### Step 3: Enable and Tune Per Cell
Enable the symbol allocation mode on the target cells and configure the required guard and control thresholds.
```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A symbolAllocMode=ENABLED
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A compactionLoadThr=30 minCompactedLength=4
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A maxCompactionUsers=4
```

#### Step 4: Verify Cell-Level Activation Status
Confirm that the symbol allocation mode is successfully running on the cell.
```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A symbolAllocMode
```

---

### Post-Change Verification
After completing the activation steps:
1. Verify that the counter `ctrCompactedAllocs` increments under light traffic conditions.
2. Confirm that the **Symbol Occupancy KPI** steps down as expected during off-peak Recording Observation Periods (ROPs).

# Cross-References
* [Parameters](parameters.md)
* [Performance Management](performance-management.md)
* [Deactivation Procedure](deactivation-procedure.md)
* [Network Impact](network-impact.md)
