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
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-30T17:37:43+00:00'
  source_sha256: 7963f633076b7c15
sources:
- resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
  title: Closed-Loop Power Control High-Band
---

This section outlines the traffic impact and command-line steps required to deactivate the Closed-Loop Power Control High-Band feature on a node.

## Traffic Impact and Operational Behavior

- **Traffic Impact:** None.
- **System Behavior:** On deactivation, accumulated loop corrections are cleared and UEs return to their open-loop operating points at their next power control update. The transition is transparent, although average uplink interference will return to pre-activation levels within minutes.
- **License Handling:** The license key (reference `LICENCE_THIS_IS_A_TEST_12345`) can remain installed for later re-activation.

## Deactivation Procedure

Deactivating the Closed-Loop Power Control High-Band feature requires a structured sequence of steps to ensure that all active carriers are gracefully returned to open-loop operation under feature control, rather than experiencing an abrupt teardown. 

The procedure consists of three main phases:
1. **Disabling the feature per sector carrier:** This step gracefully stops the closed-loop power control loop on individual sector carriers.
2. **Deactivating the feature control globally:** This step disables the feature at the node level.
3. **Verification:** This step confirms that the configuration changes have been successfully applied.

### Step 1: Disable per sector carrier

Disable the closed-loop power control functionality on the specific sector carrier (e.g., `N1A-FR2`) by setting the `closedLoopPcEnabled` parameter to `false`. This ensures that the carrier transitions back to open-loop operation gracefully.

```bash
rancli set NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled=false
```

### Step 2: Deactivate the feature

Deactivate the feature globally on the node by setting the `featureState` of `ClosedLoopPcHighBand` to `DEACTIVATED`.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=ClosedLoopPcHighBand featureState=DEACTIVATED
```

### Step 3: Verify

Verify that the closed-loop power control has been successfully disabled on the sector carrier by querying the `closedLoopPcEnabled` attribute.

```bash
rancli get NodeRoot=1,NrFunction=1,NrSectorCarrier=N1A-FR2 closedLoopPcEnabled
```

# Cross-References

- [Activation Procedure](activation-procedure.md)
