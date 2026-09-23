---
type: procedure
resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf#deactivation-procedure
title: Deactivation Procedure
description: Outlines the CLI execution steps, state verification, and traffic impact
  for deactivating Dynamic Component Carrier Management.
tags:
- dynamic-cc-management
- deactivation
- rancli
- scell
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T15:05:02+00:00'
  source_sha256: 8e574dc4c067b1c3
sources:
- title: Dynamic Component Carrier Management
  resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf
---

The Deactivation Procedure describes the steps required to disable and deactivate the Dynamic Component Carrier Management feature using `rancli` commands, along with the expected behavior and traffic impact.

## Overview and Traffic Impact

- **Traffic impact:** None.
- **SCell selection behavior:** On deactivation, SCell selection reverts to the static priority order for new setups. Existing SCells remain where they are and drift back to a static distribution as connections turn over.
- **Reconfigurations:** No forced reconfigurations are issued.
- **Post-deactivation expectation:** The per-carrier load spread is expected to gradually widen back toward the pre-activation pattern. PM data should be retained if the deactivation is part of an A/B evaluation.

## Deactivation Steps

1. **Disable the function**  
   Halts scoring and rebalancing immediately:
   ```bash
   rancli set NodeRoot=1,NrFunction=1 dynamicCcMgmtEnabled=false
   ```

2. **Deactivate the feature**  
   Deactivates the feature control:
   ```bash
   rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=DynamicCcMgmt featureState=DEACTIVATED
   ```

3. **Verify**  
   Verifies the feature control state:
   ```bash
   rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=DynamicCcMgmt featureState
   ```

# Cross-References

- [Activation Procedure](activation-procedure.md)
- [Performance Management](performance-management.md)
