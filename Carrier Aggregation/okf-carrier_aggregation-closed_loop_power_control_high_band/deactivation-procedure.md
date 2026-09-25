---
type: procedure
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#deactivation-procedure
title: Deactivation Procedure
description: Step-by-step procedure and traffic impact for deactivating the Closed-Loop
  Power Control High-Band feature.
tags:
- closed-loop-pc
- deactivation
- rancli
- nr-sector-carrier
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-25T16:49:46+00:00'
  source_sha256: 7963f633076b7c15
sources:
- title: Closed-Loop Power Control High-Band
  resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
---

This section outlines the traffic impact and command-line steps required to deactivate the Closed-Loop Power Control High-Band feature on a node.

## Traffic Impact and Operational Behavior

- **Traffic Impact:** None.
- **System Behavior:** On deactivation, accumulated loop corrections are cleared and UEs return to their open-loop operating points at their next power control update. The transition is transparent, although average uplink interference will return to pre-activation levels within minutes.
- **License Handling:** The license key can remain installed for later re-activation.

## Deactivation Procedure

Deactivate per sector carrier first (step 1), then deactivate the feature control (step 2) so that every carrier is returned to open-loop operation under feature control rather than by teardown. Step 3 verifies that the loop is disabled.

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

- [Activation Procedure](activation-procedure.md)
