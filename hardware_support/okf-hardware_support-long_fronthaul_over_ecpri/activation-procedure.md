---
type: procedure
resource: data/vodafone-mvp/raw/Long Fronthaul over eCPRI.pdf#activation-procedure
title: Activation Procedure
description: Step-by-step procedure to activate the Long Fronthaul over eCPRI feature,
  configure delay budgets, and verify operation.
tags:
- eCPRI
- Fronthaul
- Activation
- CLI
- Configuration
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:45:01+00:00'
  source_sha256: 1aaf6f8c5f5b3445
sources:
- resource: data/vodafone-mvp/raw/Long Fronthaul over eCPRI.pdf
  title: Long Fronthaul over eCPRI
---

This document describes the step-by-step procedure to activate the Long Fronthaul over eCPRI feature on a fronthaul port. It outlines the preconditions, recommended rollout strategy, execution steps, and verification commands required to configure the delay budget and ensure correct operation.

## Traffic Impact and Execution Window

* **Cell Restart:** Cells on the affected fronthaul port are restarted once during activation (resulting in an outage of typically 3–10 minutes per port). This occurs because the HARQ timing configuration and processing pipeline scheduling change.
* **Execution Window:** The activation procedure must be executed within a designated maintenance window.

## Preconditions

Prior to starting the activation, ensure the following preconditions are met:
* License key **FAK-51050** is installed on the node.
* Transport design is validated (delay, jitter, and asymmetry measured on both nominal and protection paths).
* Radio hardware is confirmed as generation **R2** or later.
* Synchronization plan is in place at the radio sites.

## Recommended Rollout Strategy

1. Activate and test on a single hub-to-site path first.
2. Complete full acceptance testing, including performing a forced optical protection switch.
3. Roll out the configuration to the remaining paths once the first path is verified.

---

## Step-by-Step Activation Procedure

### Step 1: Verify the License Key is Installed
Query the node to verify that the required license is enabled:

```bash
rancli get NodeRoot=1,EquipmentFunction=1,FeatureCtrl=LongFronthaulEcpri licenseState
```

**Expected Result:**
```text
licenseState=ENABLED (key FAK-51050)
```

### Step 2: Activate the Feature
Set the feature state to activated:

```bash
rancli set NodeRoot=1,EquipmentFunction=1,FeatureCtrl=LongFronthaulEcpri featureState=ACTIVATED
```

### Step 3: Configure the Delay Budget
Configure the delay budget parameters on the specific fronthaul port (e.g., `FH-1`) using the validated transport design values:

```bash
rancli set NodeRoot=1,EquipmentFunction=1,FronthaulPort=FH-1 maxOneWayDelay=190 jitterBudget=5 delayDriftTolerance=5
```

### Step 4: Enable Long Fronthaul Mode
Enable the long-fronthaul mode on the port. This action triggers the initial delay measurement, HARQ re-budgeting, and a restart of the associated cells:

```bash
rancli set NodeRoot=1,EquipmentFunction=1,FronthaulPort=FH-1 longFronthaulMode=ENABLED
```

### Step 5: Verify Measured Delay and Margin
Query the port to verify that the measured delay is within the budgeted limits and the validation passes:

```bash
rancli get NodeRoot=1,EquipmentFunction=1,FronthaulPort=FH-1 measuredOneWayDelay budgetValidationResult
```

**Expected Result:**
```text
measuredOneWayDelay ~187 µs, budgetValidationResult=PASS
```

---

## Post-Activation Monitoring

After the maintenance window has concluded, monitor the performance of the system to ensure stability:
* Verify that the `ctrBufferUnderruns` counter remains at zero through the next busy hour.

# Cross-References

* [Feature Dependencies](feature-depedencies.md) — For licensing and hardware prerequisite details.
* [Network Impact](network-impact.md) — Details on HARQ timing adjustments and cell outages during restart.
* [Parameters](parameters.md) — Reference for parameters such as `maxOneWayDelay`, `jitterBudget`, and `delayDriftTolerance`.
* [Deactivation Procedure](deactivation-procedure.md) — Steps to roll back or disable the feature if required.
