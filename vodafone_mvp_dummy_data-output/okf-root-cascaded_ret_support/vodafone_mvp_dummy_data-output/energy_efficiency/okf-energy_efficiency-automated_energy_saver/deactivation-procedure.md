---
type: procedure
resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf#deactivation-procedure
title: Deactivation Procedure
description: Step-by-step CLI procedure for deactivating the Automated Energy Saver
  feature and restoring subordinate feature configurations.
tags:
- deactivation
- automated-energy-saver
- rancli
- node-management
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T14:00:28+00:00'
  source_sha256: 7bfcd007cb2239e5
sources:
- resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf
  title: Automated Energy Saver
---

The Deactivation Procedure details the steps and commands required to deactivate the Automated Energy Saver feature and restore threshold ownership to subordinate features.

## Overview and Traffic Impact

* **Traffic Impact:** None.
* **Connected Users:** Unaffected.
* **Cell Lock:** No cell lock is required.
* **Wake-Up Duration:** Wake-up completes within the wake-up times of the subordinate features (seconds for micro/branch sleep, up to 2 minutes for radio deep sleep).

On deactivation, the engine first issues wake requests for every automated sleep state it owns, then returns threshold ownership to the subordinate features, which resume operating on their own configured values.

> **Pre-deactivation Note:** Review the subordinate features' statically configured thresholds before deactivating — they may be stale if the node ran in AUTO for a long time.

## Execution Steps

1. **Disable automated control** (triggers ordered wake and release):
   ```bash
   rancli set NodeRoot=1,NrFunction=1 energySaverMode=OFF
   ```
   *Description:* Moves the node to OFF, which triggers the ordered wake-and-release sequence.

2. **Deactivate the feature**:
   ```bash
   rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=AutomatedEnergySaver featureState=DEACTIVATED
   ```
   *Description:* Deactivates the feature control.

3. **Verify subordinate features run on their own configuration**:
   ```bash
   rancli get NodeRoot=1,NrFunction=1 energySaverMode
   rancli get NodeRoot=1,NrFunction=1,NrCell=N1A sleepMode
   ```
   *Description:* Verifies that the subordinate features report their own configured thresholds as active again.

# Cross-References

* [Activation Procedure](activation-procedure.md)
