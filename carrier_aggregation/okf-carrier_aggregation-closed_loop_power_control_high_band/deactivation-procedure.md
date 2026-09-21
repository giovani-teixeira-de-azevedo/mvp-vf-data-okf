---
type: procedure
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#deactivation-procedure
title: Deactivation Procedure
description: Step-by-step instructions to disable Closed-Loop Power Control High-Band
  on a per-carrier and feature-wide basis using CLI commands.
tags:
- deactivation
- rancli
- configuration
- closed-loop-pc
- high-band
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:37:03+00:00'
  source_sha256: 7963f633076b7c15
sources:
- title: Closed-Loop Power Control High-Band
  resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
---

This document describes the step-by-step deactivation procedure for the Closed-Loop Power Control High-Band feature. Deactivating this feature reverts UEs to open-loop operating points and clears any accumulated loop corrections.

## Impact and Overview

* **Traffic Impact:** None.
* **Transition Behavior:** The transition to open-loop operation is transparent. The average uplink interference returns to pre-activation levels within minutes of deactivation.
* **Procedural Note:** It is important to disable the feature per sector carrier first (Step 1), and then deactivate the main feature control (Step 2). This process ensures that every carrier returns to open-loop operation systematically under feature control rather than via teardown. The license key can remain installed for later re-activation.

---

## Step-by-Step Deactivation

### Step 1: Disable Per Sector Carrier
Disable closed-loop power control on the specific sector carrier:

```bash
rancli set NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled=false
```

### Step 2: Deactivate the Feature
Deactivate the feature control to stop the overall feature operation:

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=ClosedLoopPcHighBand featureState=DEACTIVATED
```

### Step 3: Verify Deactivation
Verify that the closed-loop power control has been disabled on the sector carrier:

```bash
rancli get NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled
```

# Cross-References

* [Activation Procedure](activation-procedure.md)
* [Feature Overview](feature-overview.md)
