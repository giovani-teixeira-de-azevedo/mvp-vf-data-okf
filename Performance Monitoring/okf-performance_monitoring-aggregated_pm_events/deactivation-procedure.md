---
type: procedure
resource: data/vodafone-mvp/raw/Aggregated PM Events.pdf#deactivation-procedure
title: DEACTIVATION PROCEDURE
description: Step-by-step procedure to deactivate the Aggregated PM Events feature
  and revert the node to raw event emission.
tags:
- deactivation
- pm-events
- rancli
- configuration
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-02T14:06:22+00:00'
  source_sha256: 78e679ebaad0488b
sources:
- title: Aggregated PM Events
  resource: data/vodafone-mvp/raw/Aggregated PM Events.pdf
---

The deactivation procedure details the steps required to safely disable the Aggregated PM Events feature on a node, reverting it to raw event emission.

## Traffic and Network Impact

Deactivating the feature has **no traffic impact**. No user-plane or control-plane functions are affected. 

However, deactivation immediately reverts the node to raw event emission for all event types. This means the PM event file volume will return to its pre-activation level starting from the next Recording Observation Period (ROP). 

> [!IMPORTANT]
> Before deactivating a large cluster of nodes simultaneously, verify that the O&M transport network and downstream storage systems are properly dimensioned to handle the increased volume of raw PM event files.

## Deactivation Steps

To ensure that in-flight accumulators are flushed as final records rather than being discarded, follow the steps below in order:

### Step 1: Disable Each Aggregation Group
Disable each aggregation group first. This flushes any in-flight accumulators as final records.

```bash
rancli set NodeRoot=1,NrFunction=1,PmEventAggregation=1 aggregationState=DISABLED
```

### Step 2: Deactivate the Feature Control
Deactivate the main feature control. The license key can remain installed on the node for potential future re-activation.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=AggregatedPmEvents featureState=DEACTIVATED
```

### Step 3: Verify Deactivation
Verify that the feature state has been successfully updated to deactivated.

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=AggregatedPmEvents featureState
```

# Cross-References

* [Activation Procedure](activation-procedure.md) - Steps to activate the Aggregated PM Events feature.
* [Parameters](parameters.md) - Reference for configuration parameters such as `aggregationState` and `featureState`.
* [Feature Overview](feature-overview.md) - Overview of the Aggregated PM Events feature.
