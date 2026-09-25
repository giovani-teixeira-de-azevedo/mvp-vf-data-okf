---
type: procedure
resource: data/vodafone-mvp/raw/NR Massive MIMO Sleep Mode.pdf#deactivation-procedure
title: DEACTIVATION PROCEDURE
description: Details the deactivation sequence, traffic impact, and CLI commands for
  disabling the NR Massive MIMO Sleep Mode feature.
tags:
- ran
- nr
- massive-mimo
- sleep-mode
- deactivation
- cli
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-25T16:09:55+00:00'
  source_sha256: 14bd74a39a5754e5
sources:
- resource: data/vodafone-mvp/raw/NR Massive MIMO Sleep Mode.pdf
  title: NR Massive MIMO Sleep Mode
---

The **Deactivation Procedure** section defines the operational steps, execution sequence, traffic impact, and CLI commands required to disable the NR Massive MIMO Sleep Mode feature per cell and globally on a gNodeB.

## Operational Overview

- **Traffic Impact:** None. If a cell is asleep at the moment of deactivation, full branch operation is restored first. Wake-up completes within approximately 2 seconds and is transparent to connected UEs, which simply regain full beamforming gain.
- **Cell Locking:** No cell lock is required.
- **Execution Order:** Deactivation must be performed per cell first (Step 1) before deactivating the global feature control (Step 2). This sequence ensures every cell returns to full operation under feature control rather than being forced awake by feature teardown.
- **Verification:** Step 3 verifies that the radio reports the full active branch count.
- **License Retention:** The license key can remain installed. It is unused while `featureState=DEACTIVATED`, enabling simplified two-command re-activation.

## Command Sequence

```bash
# 1. Disable per cell 
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A sleepMode=DISABLED 
 
# 2. Deactivate the feature 
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=MassiveMimoSleep featureState=DEACTIVATED 
 
# 3. Verify all branches are active 
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A activeBranchCount
```

# Cross-References

- [Activation Procedure](activation-procedure.md)
