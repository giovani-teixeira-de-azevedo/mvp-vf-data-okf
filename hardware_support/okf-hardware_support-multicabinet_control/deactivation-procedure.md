---
type: procedure
resource: data/vodafone-mvp/raw/Multicabinet Control.pdf#deactivation-procedure
title: Deactivation Procedure
description: Describes the step-by-step technical procedure and CLI commands to deactivate
  the Multicabinet Control feature.
tags:
- deactivation
- configuration
- cli
- rancli
- multicabinet-control
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:46:09+00:00'
  source_sha256: 09264dfa2acb02cb
sources:
- title: Multicabinet Control
  resource: data/vodafone-mvp/raw/Multicabinet Control.pdf
---

This section describes the procedure to deactivate the Multicabinet Control feature within the node configuration. It outlines the impacts of deactivation, local cabinet behavior post-deactivation, and the sequence of CLI commands required to complete the process.

## Impact Analysis

*   **Traffic Impact:** None.
*   **Supervision Impact:** Real impact. Secondary cabinets revert to autonomous local control, which is safe but unmonitored. Alternative supervision (or acceptance of the monitoring gap) must be agreed upon before deactivating on staffed-alarm sites.

## System Behavior After Unbinding

Secondary Support Control Units (SCUs) retain their last synchronized setpoints and all local safety protections after unbinding, ensuring that the physical equipment remains protected. 

The license key can remain installed to allow for later re-activation, which will restore the cabinet bindings from the retained configuration.

## Deactivation Steps

Deactivation is executed in three main steps:

1.  **Unbind secondary cabinets:** This step unbinds each secondary cabinet (repeat the command for each secondary cabinet in the configuration).
2.  **Deactivate the feature control.**
3.  **Verify primary cabinet supervision:** Confirm that the primary cabinet's own supervision remains unaffected.

### CLI Command Sequence

#### Step 1: Unbind Secondary Cabinets

Run the following commands to clear the `boundScuSerial` parameter for each secondary cabinet:

```bash
rancli set NodeRoot=1,EquipmentFunction=1,Cabinet=2 boundScuSerial="" 
rancli set NodeRoot=1,EquipmentFunction=1,Cabinet=3 boundScuSerial="" 
```

#### Step 2: Deactivate the Feature

Deactivate the feature control using the following command:

```bash
rancli set NodeRoot=1,EquipmentFunction=1,FeatureCtrl=MulticabinetControl featureState=DEACTIVATED 
```

#### Step 3: Verify Primary Cabinet Supervision

Query the primary cabinet status to verify that its own operational and environmental supervision continues to function:

```bash
rancli get NodeRoot=1,EquipmentFunction=1,Cabinet=1 operationalState currentTemperature
```

# Cross-References

*   [Activation Procedure](activation-procedure.md)
*   [Feature Overview](feature-overview.md)
*   [Parameters](parameters.md)
