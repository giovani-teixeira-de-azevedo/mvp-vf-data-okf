---
type: procedure
resource: data/vodafone-mvp/raw/NR Massive MIMO Sleep Mode.pdf#deactivation-procedure
title: Deactivation Procedure
description: Step-by-step instructions to disable NR Massive MIMO Sleep Mode per cell
  and deactivate the feature control.
tags:
- massive-mimo
- sleep-mode
- deactivation
- rancli
- procedures
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:42:13+00:00'
  source_sha256: 14bd74a39a5754e5
sources:
- resource: data/vodafone-mvp/raw/NR Massive MIMO Sleep Mode.pdf
  title: NR Massive MIMO Sleep Mode
---

This section details the step-by-step procedure to safely deactivate the NR Massive MIMO Sleep Mode feature. The deactivation process is designed to have zero traffic impact and does not require a cell lock.

## Deactivation Overview

- **Traffic Impact:** None. If a cell is asleep at the moment of deactivation, full branch operation is restored first. The wake-up process completes within approximately 2 seconds and is transparent to connected User Equipments (UEs), which simply regain the full beamforming gain.
- **Cell Lock Requirements:** No cell lock is required.
- **Execution Order:** Deactivation must be performed per cell first (Step 1) and only then on the feature control level (Step 2). This specific ordering guarantees that every cell returns to full operation under feature control rather than being forced awake by the feature teardown.
- **License Handling:** The license key can remain installed on the node; it is simply unused while `featureState` is set to `DEACTIVATED`, making future re-activation a simple two-command operation.

---

## Step-by-Step Procedure

### Step 1: Disable Sleep Mode Per Cell
Disable the sleep mode parameter on the target cell (e.g., `N1A`).
```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A sleepMode=DISABLED
```

### Step 2: Deactivate the Feature Control
Deactivate the global Massive MIMO Sleep Mode feature state.
```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=MassiveMimoSleep featureState=DEACTIVATED
```

### Step 3: Verify Active Branches
Verify that the radio reports the full active branch count for the cell.
```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A activeBranchCount
```

# Cross-References

* [Activation Procedure](activation-procedure.md) — For details on how to re-enable and activate the feature.
