---
type: procedure
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#deactivation-procedure
title: Deactivation Procedure
description: Outlines the procedure and commands for deactivating the Closed-Loop
  Power Control High-Band feature.
tags:
- closed-loop power control
- deactivation
- rancli
- nrsectorcarrier
- featurectrl
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-25T11:06:14+00:00'
  source_sha256: 7963f633076b7c15
sources:
- title: Closed-Loop Power Control High-Band
  resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
---

This section describes the procedure, command steps, and network impact associated with deactivating the Closed-Loop Power Control High-Band feature.

## Overview and Impact

* **Traffic Impact:** None.
* **UE Behavior:** On deactivation, accumulated loop corrections are cleared, and UEs return to their open-loop operating points at their next power control update.
* **Interference:** The transition is transparent, although average uplink interference will return to pre-activation levels within minutes.
* **Licensing:** The license key can remain installed for later re-activation.

## Deactivation Sequence

Deactivation must be performed per sector carrier first (step 1), followed by feature control deactivation (step 2) to ensure every carrier is returned to open-loop operation under feature control rather than by teardown. Step 3 verifies that the loop is disabled.

### Step 1: Disable per sector carrier

```bash
rancli set NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled=false
```

### Step 2: Deactivate the feature

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=ClosedLoopPcHighBand featureState=DEACTIVATED
```

### Step 3: Verify

```bash
rancli get NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled
```

# Cross-References

* [Activation Procedure](activation-procedure.md)
