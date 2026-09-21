---
type: procedure
resource: data/vodafone-mvp/raw/Energy-Optimized Slot Allocation.pdf#activation-procedure
title: Activation Procedure
description: Step-by-step activation procedure and commands for the Energy-Optimized
  Slot Allocation feature.
tags:
- activation
- rancli
- procedure
- configuration
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:40:18+00:00'
  source_sha256: f3bd6fe1007dbbe2
sources:
- title: Energy-Optimized Slot Allocation
  resource: data/vodafone-mvp/raw/Energy-Optimized Slot Allocation.pdf
---

The activation procedure outlines the necessary steps, commands, and prerequisites to enable and configure the Energy-Optimized Slot Allocation feature on NR cells.

## Preconditions and Considerations

*   **License Key:** License key `FAK-31535` must be installed.
*   **Feature Dependencies:** NR Micro Sleep Tx should be activated first (or simultaneously) on the same cells to ensure that the created empty slots are successfully monetized. For more information on dependencies, see [Feature Dependencies](feature-depedencies.md).
*   **Traffic & Latency Impact:** Activation introduces up to `maxBatchDelay` milliseconds of additional queuing delay for non-exempt downlink traffic at low load. This is generally imperceptible to end users but does modify the latency profile. Operators with strict latency SLAs on best-effort traffic should activate during a maintenance window and verify latency KPIs before the busy hour.
*   **Service Impact:** No cell lock or restart is required during this procedure.
*   **Recommended Rollout:** Start with a pilot cluster using default parameters. Verify Added Delay and throughput KPIs for one week before extending the rollout.

---

## Step-by-Step Activation

### Step 1: Verify the License Key Installation
Confirm that the required license key is active on the node.

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=EnergyOptSlotAlloc licenseState
```

**Expected Output:**
```text
licenseState=ENABLED (key FAK-31535)
```

### Step 2: Activate the Feature Control
Enable the feature globally or at the function level.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=EnergyOptSlotAlloc featureState=ACTIVATED
```

### Step 3: Enable and Tune Per Cell
Enable the feature on the target cell (e.g., `N1A`) and explicitly configure the delay bound, target slot fill, and allowed load threshold. For details on these parameters, refer to [Parameters](parameters.md).

```bash
# Enable slot allocation mode on the cell
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A slotAllocMode=ENABLED

# Set the maximum queuing delay (in milliseconds) and the target slot fill percentage
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A maxBatchDelay=4 targetSlotFill=85

# Set the maximum cell load threshold below which batching is allowed
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A batchingAllowedLoad=40
```

### Step 4: Verify Cell Activation Status
Query the cell configuration to verify that slot allocation has been successfully enabled.

```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A slotAllocMode
```

---

## Post-Activation Verification

Once the activation commands have been applied:
1.  **Empty Slot Ratio:** Confirm that the Empty Slot Ratio increases as expected in the subsequent off-peak Recording Observation Periods (ROPs).
2.  **Scheduling Latency:** Monitor key performance indicators to verify that the P95 scheduling latency stays within the configured `maxBatchDelay` bound. For detailed PM counter information, see [Performance Management](performance-management.md).

# Cross-References

*   [Feature Dependencies](feature-depedencies.md)
*   [Parameters](parameters.md)
*   [Performance Management](performance-management.md)
*   [Deactivation Procedure](deactivation-procedure.md)
