---
type: procedure
resource: data/vodafone-mvp/raw/Coverage-Optimized Uplink Transmission High-Band.pdf#deactivation-procedure
title: DEACTIVATION PROCEDURE
description: Step-by-step procedure to disable and deactivate the Coverage-Optimized
  Uplink Transmission High-Band feature, including cluster restart.
tags:
- deactivation
- rancli
- configuration
- coverage-optimization
- cluster-restart
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T11:20:32+00:00'
  source_sha256: b58aa8fbfc6401d5
sources:
- title: Coverage-Optimized Uplink Transmission High-Band
  resource: data/vodafone-mvp/raw/Coverage-Optimized Uplink Transmission High-Band.pdf
---

This section describes the step-by-step procedure to deactivate the Coverage-Optimized Uplink Transmission High-Band feature. It outlines the traffic impact, recommended scheduling, and the specific `rancli` commands required to disable the feature per cell, deactivate it globally, verify the status, and restart the cluster.

## Traffic Impact and Considerations

* **Traffic Impact:** None. UEs currently in coverage mode are individually reconfigured back to normal transmission format via RRC reconfiguration.
* **Transition Behavior:** The transition is transparent, though edge UEs lose the coverage benefit immediately and may subsequently drop where they previously survived.
* **Scheduling Recommendation:** Consider scheduling deactivation outside busy hours on cells with a high Coverage Mode Ratio, since edge UEs will lose robustness at the moment of deactivation.
* **License Key:** The license key can remain installed for later re-activation.

## Deactivation Steps

To ensure a clean deactivation, disable the feature per cell first (Step 1) so every UE is restored under feature control, then deactivate the feature control globally (Step 2). Verify that no UEs remain in coverage mode (Step 3), and finally, restart the cluster to complete the deactivation process (Step 4).

### Step 1: Disable Per Cell

Disable the coverage-optimized uplink transmission mode on the target cell:

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A covOptUlTxMode=DISABLED
```

### Step 2: Deactivate the Feature

Deactivate the feature state control:

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=CovOptUlTxHighBand featureState=DEACTIVATED
```

### Step 3: Verify Deactivation

Confirm that no UEs remain in coverage mode on the cell:

```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A covModeActiveUeCount
```

### Step 4: Restart the Cluster

Restart the cluster to apply the changes and complete the deactivation process:

```bash
rancli restart cluster
```

# Cross-References

* [Activation Procedure](activation-procedure.md)
* [Parameters](parameters.md)
