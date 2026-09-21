---
type: procedure
resource: data/vodafone-mvp/raw/Extended Propagation Delay Support High-Band.pdf#activation-procedure
title: Activation Procedure
description: Step-by-step procedure to activate and verify the Extended Propagation
  Delay Support High-Band feature.
tags:
- activation
- procedure
- FR2
- high-band
- CLI
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:26:55+00:00'
  source_sha256: 207fe7c9b0da30e8
sources:
- title: Extended Propagation Delay Support High-Band
  resource: data/vodafone-mvp/raw/Extended Propagation Delay Support High-Band.pdf
---

This section outlines the step-by-step activation procedure for the Extended Propagation Delay Support High-Band feature. The procedure includes preconditions, rollout recommendations, traffic impact notes, and CLI execution steps.

## Impact & Preconditions

### Traffic Impact
- **Brief Cell Restart Required:** Changing the PRACH format and receive window rebuilds the cell configuration.
- **Downtime:** Each reconfigured FR2 cell is unavailable for approximately **30–60 seconds**.
- **Scheduling:** Activation should be scheduled in a maintenance window, or rely on Carrier Aggregation (CA) and mobility to shift users to co-sited FR1 layers during the restart.

### Preconditions
Before starting the activation procedure, ensure the following requirements are met:
- License key **FAK-33014** is installed.
- **Physical Layer High-Band** is active.
- The frequency reuse plan has been reviewed for the extended footprint.
- Where the cell serves as a Secondary Cell (SCell), the CA baseline must be active.

---

## Recommended Rollout Strategy
Activate on the **FWA-serving (Fixed Wireless Access) cells first**, since they hold the distant population that justifies the protocol overhead. 

The activation flow consists of four key phases:
1. **Verify:** Verify the license state.
2. **Activate:** Activate the global feature control.
3. **Configure:** Lock the cell, apply the range configuration, and unlock the cell (locking ensures a deterministic rebuild).
4. **Verify Post-change:** Confirm configuration and check that SIB1 broadcasts the new PRACH configuration and that the performance counter begins counting.

---

## Execution Steps (CLI)

### Step 1: Verify the license key is installed
Run the following command to check the license state:
```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=ExtPropDelayHighBand licenseState
```
*Expected Output:*
```text
licenseState=ENABLED (key FAK-33014)
```

### Step 2: Activate the feature
Activate the feature state globally:
```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=ExtPropDelayHighBand featureState=ACTIVATED
```

### Step 3: Configure per cell (during maintenance window)
Lock the target cell, configure the extended range parameter settings on the sector carrier, and then unlock the cell:
```bash
rancli lock NodeRoot=1,NrFunction=1,NrCell=N1A
rancli set NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 extendedRangeMode=ENABLED maxCellRange=8000
rancli unlock NodeRoot=1,NrFunction=1,NrCell=N1A
```

### Step 4: Verify cell configuration
Verify that the parameters have been applied correctly:
```bash
rancli get NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 extendedRangeMode maxCellRange
```

### Post-Change Verification
After completion, perform the following validation:
1. Confirm that **SIB1** broadcasts the new PRACH configuration.
2. Verify that the counter `ctrRaAttemptExtRange` starts counting.

---

# Cross-References
* [Parameters](parameters.md) - For details on parameters like `extendedRangeMode` and `maxCellRange`.
* [Performance Management](performance-management.md) - For details on the `ctrRaAttemptExtRange` counter.
* [Deactivation Procedure](deactivation-procedure.md) - Instructions to revert these configuration changes.
