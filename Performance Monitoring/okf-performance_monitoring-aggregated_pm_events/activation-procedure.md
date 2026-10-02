---
type: procedure
resource: data/vodafone-mvp/raw/Aggregated PM Events.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Step-by-step procedure to activate and configure the Aggregated PM Events
  feature.
tags:
- activation
- procedure
- configuration
- pm-events
- rancli
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-02T14:06:25+00:00'
  source_sha256: a754bebeecebcdf4
sources:
- resource: data/vodafone-mvp/raw/Aggregated PM Events.pdf
  title: Aggregated PM Events
---

This section describes the step-by-step procedure to activate and configure the Aggregated PM Events feature.

## Traffic Impact & Preconditions

### Traffic Impact
There is **no traffic impact**. The feature operates purely in the O&M plane; no cell lock, restart, or maintenance window is required. Aggregated records begin to appear in the next ROP (Recording Observation Period) file after group enablement.

### Preconditions
* **License Key:** License key `FAK-30110` must be installed.
* **Northbound Mediation:** The northbound mediation system must be upgraded to a release that parses aggregated record format version 3.

### Recommended Rollout Order
1. Enable one aggregation group (per-cell dimension) on a pilot node.
2. Run both raw pass-through and aggregation in parallel for one week (`rawPassThrough=true`).
3. Validate downstream KPI equivalence.
4. Disable pass-through and extend to further nodes and finer dimensions.

---

## Step-by-Step Activation

### Step 1: Verify the license key is installed
Verify that the license key is installed and enabled before proceeding with configuration.

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=AggregatedPmEvents licenseState
```

**Expected Output:**
```text
licenseState=ENABLED (key FAK-30110)
```

### Step 2: Activate the feature
Activate the feature control.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=AggregatedPmEvents featureState=ACTIVATED
```

### Step 3: Configure an aggregation group
Create and configure an aggregation group. In this example, the aggregation dimension is set to `CELL_5QI`, the aggregation period is set to `60` minutes, raw pass-through is enabled, and specific event types are configured.

```bash
rancli set NodeRoot=1,NrFunction=1,PmEventAggregation=1 aggregationDimension=CELL_5QI
rancli set NodeRoot=1,NrFunction=1,PmEventAggregation=1 aggregationPeriod=60 rawPassThrough=true
rancli set NodeRoot=1,NrFunction=1,PmEventAggregation=1 eventTypeList="RRC_SETUP_RESULT,DL_UE_THP_SAMPLE"
```

### Step 4: Enable the group
Enable the configured aggregation group.

```bash
rancli set NodeRoot=1,NrFunction=1,PmEventAggregation=1 aggregationState=ENABLED
```

### Step 5: Verify the state
Verify that the aggregation group state is enabled.

```bash
rancli get NodeRoot=1,NrFunction=1,PmEventAggregation=1 aggregationState
```

### Post-Activation Verification
After the next ROP closes, confirm that the performance counters behave as expected:
* `ctrAggRecordsEmitted` is incrementing.
* `ctrAggRecordsDropped` remains zero.

# Cross-References

* [Feature Dependencies](feature-depedencies.md) - Details on license keys and feature dependencies.
* [Parameters](parameters.md) - Reference for parameters like `aggregationDimension`, `aggregationPeriod`, and `rawPassThrough`.
* [Performance Management](performance-management.md) - Details on performance counters such as `ctrAggRecordsEmitted` and `ctrAggRecordsDropped`.
* [Deactivation Procedure](deactivation-procedure.md) - Steps to deactivate the feature.
