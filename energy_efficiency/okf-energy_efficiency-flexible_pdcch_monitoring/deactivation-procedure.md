---
type: procedure
resource: data/vodafone-mvp/raw/Flexible PDCCH Monitoring.pdf#deactivation-procedure
title: DEACTIVATION PROCEDURE
description: Procedure for deactivating the Flexible PDCCH Monitoring feature and
  disabling it per cell.
tags:
- deactivation
- pdcch-monitoring
- configuration
- rancli
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:41:47+00:00'
  source_sha256: aa9b2ae7efac70ee
sources:
- title: Flexible PDCCH Monitoring
  resource: data/vodafone-mvp/raw/Flexible PDCCH Monitoring.pdf
---

This section describes the procedure for disabling and deactivating the Flexible PDCCH Monitoring feature. It outlines the system behavior, transition mechanisms, traffic impact, and step-by-step CLI commands required to restore legacy static-monitoring behavior.

## Traffic and Transition Impact

- **Traffic Impact**: None.
- **System Behavior on Deactivation**:
  - No new switch or skip indications are issued.
  - UEs currently in the sparse group are switched back to dense monitoring immediately via Downlink Control Information (DCI).
  - The Search Space Set Group (SSSG) configuration is removed from UEs at their next RRC reconfiguration.
  - From that point onward, all UEs behave as legacy static-monitoring UEs.

---

## Deactivation Steps

The deactivation process consists of disabling the function per cell to trigger the dense-group recall, deactivating the feature control, and verifying the status in the next ROP (Recording Observation Period).

### Step 1: Disable Per Cell (Recalls UEs to Dense Monitoring)
Disable the PDCCH monitoring mode on the target cell. This command triggers the recall of all UEs to dense monitoring.

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A pdcchMonitoringMode=DISABLED
```

### Step 2: Deactivate the Feature Control
Deactivate the overall Flexible PDCCH Monitoring feature.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=FlexPdcchMonitoring featureState=DEACTIVATED
```

### Step 3: Verify Configuration
Verify that the PDCCH monitoring mode has been successfully disabled. Confirm that sparse time stops accumulating in the following ROP.

```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A pdcchMonitoringMode
```

# Cross-References

* [Activation Procedure](activation-procedure.md)
* [Parameters](parameters.md)
