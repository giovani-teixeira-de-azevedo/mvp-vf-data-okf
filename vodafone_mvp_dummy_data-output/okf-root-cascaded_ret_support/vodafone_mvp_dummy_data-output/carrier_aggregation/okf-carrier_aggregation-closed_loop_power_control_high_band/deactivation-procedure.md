---
type: procedure
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#deactivation-procedure
title: Deactivation Procedure
description: Details the step-by-step procedure, traffic impact, and CLI commands
  for deactivating Closed-Loop Power Control High-Band.
tags:
- rancli
- closed-loop-pc
- deactivation
- power-control
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T14:51:48+00:00'
  source_sha256: 7963f633076b7c15
sources:
- title: Closed-Loop Power Control High-Band
  resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
---

This section details the deactivation procedure for the Closed-Loop Power Control High-Band feature, including traffic impact, operation behavior, and step-by-step CLI commands.

## Overview and Traffic Impact

* **Traffic Impact:** None.
* **Operating Behavior:** On deactivation, accumulated loop corrections are cleared and UEs return to their open-loop operating points at their next power control update. The transition is transparent, although average uplink interference will return to pre-activation levels within minutes.
* **Order of Execution:** Deactivate per sector carrier first (Step 1), then deactivate the feature control (Step 2) so that every carrier is returned to open-loop operation under feature control rather than by teardown. Step 3 verifies the loop is disabled.
* **License Handling:** The license key can remain installed for later re-activation.

## Step-by-Step Procedure

### Step 1. Disable Per Sector Carrier

```bash
rancli set NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled=false
```

### Step 2. Deactivate the Feature

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=ClosedLoopPcHighBand featureState=DEACTIVATED
```

### Step 3. Verify

```bash
rancli get NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled
```

# Cross-References

* [Activation Procedure](activation-procedure.md)
