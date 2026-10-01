---
type: procedure
resource: data/vodafone-mvp/raw/Downlink Throughput Test in gNodeB.pdf#deactivation-procedure
title: Deactivation Procedure
description: Step-by-step instructions to deactivate the Downlink Throughput Test
  feature in gNodeB.
tags:
- gNodeB
- Downlink Throughput Test
- Deactivation
- rancli
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T10:45:43+00:00'
  source_sha256: 05be935b8738d405
sources:
- resource: data/vodafone-mvp/raw/Downlink Throughput Test in gNodeB.pdf
  title: Downlink Throughput Test in gNodeB
---

This section describes the procedure to deactivate the Downlink Throughput Test feature in the gNodeB, including traffic impact considerations and step-by-step commands.

## Traffic Impact

There is no traffic impact associated with deactivating this feature:
* Any test in progress is aborted cleanly at deactivation.
* The padding flow is deregistered from the scheduler within one scheduling period.
* The affected User Equipment (UE) simply stops receiving test data.
* Other connected users are unaffected.

## Pre-Deactivation Recommendations

* **Stop ongoing tests explicitly first (Step 1):** This ensures that partial records are flushed with the cause `OPERATOR_STOP` rather than `FEATURE_DEACTIVATED`, which keeps test statistics clean.
* **Export test records:** Retained test records remain readable until overwritten. Export them before deactivation if they are needed for reporting.

## Deactivation Steps

### 1. Stop any ongoing tests
Execute the following command to stop all active tests:

```bash
rancli action NodeRoot=1,NrFunction=1,DlThpTest=1 stopAllTests
```

### 2. Deactivate the feature
Set the feature state to `DEACTIVATED`:

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=DlThpTest featureState=DEACTIVATED
```

### 3. Verify
Verify that the feature state has been successfully updated:

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=DlThpTest featureState
```

# Cross-References

* [Activation Procedure](activation-procedure.md)
* [Feature Operation](feature-operation.md)
* [Performance Management](performance-management.md)
