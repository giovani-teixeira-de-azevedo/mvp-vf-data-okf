---
type: procedure
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#deactivation-procedure
title: DEACTIVATION PROCEDURE
description: Step-by-step procedure and traffic impact details for deactivating the
  Closed-Loop Power Control High-Band feature.
tags:
- closed-loop-power-control
- deactivation
- rancli
- nr
- high-band
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-24T14:33:04+00:00'
  source_sha256: 7963f633076b7c15
sources:
- title: Closed-Loop Power Control High-Band
  resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
---

This section details the step-by-step procedure and traffic impact associated with deactivating the Closed-Loop Power Control High-Band feature.

## Traffic Impact and Deactivation Behavior

* **Traffic Impact:** None.
* **UE Operating Point:** On deactivation, accumulated loop corrections are cleared and UEs return to their open-loop operating points at their next power control update.
* **Transition & Interference:** The transition is transparent, although average uplink interference will return to pre-activation levels within minutes.
* **Order of Execution:** Deactivate per sector carrier first (Step 1), then deactivate the feature control (Step 2) so that every carrier is returned to open-loop operation under feature control rather than by teardown. Step 3 verifies the loop is disabled.
* **License Key:** The license key can remain installed for later re-activation.

## Procedure Steps

### 1. Disable Per Sector Carrier

```bash
rancli set NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled=false
```

### 2. Deactivate the Feature

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=ClosedLoopPcHighBand featureState=DEACTIVATED
```

### 3. Verify

```bash
rancli get NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled
```

# Cross-References

* [ACTIVATION PROCEDURE](activation-procedure.md)
* [FEATURE OVERVIEW](feature-overview.md)
