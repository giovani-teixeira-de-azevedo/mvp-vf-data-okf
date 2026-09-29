---
type: procedure
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#deactivation-procedure
title: Deactivation Procedure
description: Step-by-step procedure and traffic impact for deactivating Closed-Loop
  Power Control High-Band.
tags:
- rancli
- closed-loop-power-control
- deactivation
- nr-sector-carrier
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-29T13:04:39+00:00'
  source_sha256: 7963f633076b7c15
sources:
- resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
  title: Closed-Loop Power Control High-Band
---

This section outlines the traffic impact and command-line steps required to deactivate the Closed-Loop Power Control High-Band feature.

## Traffic Impact

* **Impact:** None.
* **Behavior:** On deactivation, accumulated loop corrections are cleared and UEs return to their open-loop operating points at their next power control update. The transition is transparent, although average uplink interference will return to pre-activation levels within minutes.

## Procedure Steps

1. **Disable per sector carrier**  
   Disable the feature on the sector carrier first so that every carrier is returned to open-loop operation under feature control rather than by teardown:
   ```bash
   rancli set NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled=false
   ```

2. **Deactivate the feature**  
   Deactivate the feature control:
   ```bash
   rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=ClosedLoopPcHighBand featureState=DEACTIVATED
   ```

3. **Verify**  
   Verify that the loop is disabled:
   ```bash
   rancli get NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled
   ```

Note that the license key can remain installed for later re-activation.
