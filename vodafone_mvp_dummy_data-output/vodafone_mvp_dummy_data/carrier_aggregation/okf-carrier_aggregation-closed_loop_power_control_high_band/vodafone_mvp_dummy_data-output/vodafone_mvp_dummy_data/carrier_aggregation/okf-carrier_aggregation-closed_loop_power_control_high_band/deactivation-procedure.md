---
type: procedure
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#deactivation-procedure
title: Deactivation Procedure
description: Describes the traffic impact, execution steps, and commands for deactivating
  the Closed-Loop Power Control High-Band feature.
tags:
- closed-loop power control
- deactivation
- rancli
- nrsectorcarrier
- featurectrl
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T12:53:58+00:00'
  source_sha256: 7963f633076b7c15
sources:
- resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
  title: Closed-Loop Power Control High-Band
---

This section details the deactivation procedure for the Closed-Loop Power Control High-Band feature, including traffic impact, step order, and CLI execution commands.

## Overview and Impact

- **Traffic impact:** None.
- **Behavior on deactivation:**
  - Accumulated loop corrections are cleared.
  - User Equipments (UEs) return to their open-loop operating points at their next power control update.
  - The transition is transparent, although average uplink interference returns to pre-activation levels within minutes.
- **Licensing:** The license key can remain installed for later re-activation.

## Deactivation Steps

Deactivate per sector carrier first (step 1), then deactivate the feature control (step 2) so that every carrier is returned to open-loop operation under feature control rather than by teardown. Step 3 verifies that the loop is disabled.

### Step 1: Disable per sector carrier

```text
rancli set NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled=false
```

### Step 2: Deactivate the feature

```text
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=ClosedLoopPcHighBand featureState=DEACTIVATED
```

### Step 3: Verify

```text
rancli get NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled
```

# Cross-References

- [Activation Procedure](activation-procedure.md)
