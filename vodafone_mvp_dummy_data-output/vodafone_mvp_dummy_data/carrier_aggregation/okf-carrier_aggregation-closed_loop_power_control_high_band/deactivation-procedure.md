---
type: procedure
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#deactivation-procedure
title: DEACTIVATION PROCEDURE
description: Step-by-step procedure and traffic impact details for deactivating the
  Closed-Loop Power Control High-Band feature.
tags:
- rancli
- deactivation
- closed-loop-power-control
- nr-sector-carrier
- feature-control
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T15:13:03+00:00'
  source_sha256: 7963f633076b7c15
sources:
- title: Closed-Loop Power Control High-Band
  resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
---

This section details the deactivation procedure and associated traffic impact for the Closed-Loop Power Control High-Band feature within the network configuration.

## Overview and Impact

* **Traffic impact:** None.
* **UE behavior:** On deactivation, accumulated loop corrections are cleared and UEs return to their open-loop operating points at their next power control update.
* **Interference impact:** The transition is transparent, although average uplink interference will return to pre-activation levels within minutes.
* **License management:** The license key can remain installed for later re-activation.

## Deactivation Steps

Deactivation must be performed per sector carrier first (step 1), followed by deactivating the feature control (step 2) so that every carrier is returned to open-loop operation under feature control rather than by teardown. Step 3 verifies that the loop is disabled.

### 1. Disable per sector carrier

```bash
rancli set NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled=false
```

### 2. Deactivate the feature

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=ClosedLoopPcHighBand featureState=DEACTIVATED
```

### 3. Verify

```bash
rancli get NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled
```

# Cross-References

* [Activation Procedure](activation-procedure.md)
