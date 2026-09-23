---
type: procedure
resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf#deactivation-procedure
title: Deactivation Procedure
description: Step-by-step deactivation procedure and operational impact for Automated
  Energy Saver.
tags:
- Automated Energy Saver
- Deactivation Procedure
- rancli
- energySaverMode
- featureState
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T18:17:55+00:00'
  source_sha256: 7bfcd007cb2239e5
sources:
- title: Automated Energy Saver
  resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf
---

The deactivation procedure disables the Automated Energy Saver feature, returning threshold control to subordinate features while restoring normal operating states without affecting active users.

## Traffic Impact and Deactivation Behavior

* **Traffic impact:** None.
* **Cell lock:** No cell lock is required.
* **Wake-and-release sequence:** On deactivation, the engine first issues wake requests for every automated sleep state it owns, then returns threshold ownership to the subordinate features, which resume operating on their own configured values.
* **Wake-up times:** Wake-up completes within the wake-up times of the subordinate features (seconds for micro/branch sleep, up to 2 minutes for radio deep sleep). Connected users are unaffected.
* **Pre-deactivation check:** Review the subordinate features' statically configured thresholds before deactivating, as they may be stale if the node ran in `AUTO` mode for a long time.

## Execution Procedure

### Step 1: Disable Automated Control
Move the node to `OFF`, which triggers the ordered wake-and-release sequence.

```bash
rancli set NodeRoot=1,NrFunction=1 energySaverMode=OFF
```

### Step 2: Deactivate the Feature
Deactivate the feature control object.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=AutomatedEnergySaver featureState=DEACTIVATED
```

### Step 3: Verify Subordinate Feature Configuration
Verify that the subordinate features report their own configured thresholds as active again.

```bash
rancli get NodeRoot=1,NrFunction=1 energySaverMode
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A sleepMode
```

## Cross-References

* [Activation Procedure](activation-procedure.md)
