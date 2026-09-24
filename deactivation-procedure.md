---
type: procedure
resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf#deactivation-procedure
title: DEACTIVATION PROCEDURE
description: Outlines the deactivation procedure, traffic impact, wake-and-release
  sequence, and CLI commands for the Automated Energy Saver feature.
tags:
- deactivation
- automated-energy-saver
- rancli
- energySaverMode
- featureState
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-24T08:37:37+00:00'
  source_sha256: 7bfcd007cb2239e5
sources:
- title: Automated Energy Saver
  resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf
---

This section describes the deactivation procedure for the Automated Energy Saver feature, detailing its traffic impact, wake-and-release sequence, pre-deactivation considerations, and CLI execution steps.

## Deactivation Overview and Traffic Impact

Deactivation of the Automated Energy Saver feature has **no traffic impact**, requires no cell lock, and leaves connected users unaffected.

During deactivation:
1. The engine issues wake requests for every automated sleep state it owns.
2. Threshold ownership is returned to subordinate features, which resume operating on their own configured values.
3. Wake-up completes within the wake-up times of the subordinate features (seconds for micro/branch sleep, up to 2 minutes for radio deep sleep).

Before deactivating, review the subordinate features' statically configured thresholds, as they may be stale if the node has been running in `AUTO` for a long duration.

## Procedure Steps

1. **Disable automated control** (triggers the ordered wake-and-release sequence):
   ```bash
   rancli set NodeRoot=1,NrFunction=1 energySaverMode=OFF
   ```

2. **Deactivate the feature control**:
   ```bash
   rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=AutomatedEnergySaver featureState=DEACTIVATED
   ```

3. **Verify that subordinate features report their own configured thresholds as active again**:
   ```bash
   rancli get NodeRoot=1,NrFunction=1 energySaverMode
   rancli get NodeRoot=1,NrFunction=1,NrCell=N1A sleepMode
   ```

# Cross-References

- [Activation Procedure](activation-procedure.md)
