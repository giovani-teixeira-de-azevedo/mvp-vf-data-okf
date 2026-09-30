---
type: procedure
resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf#deactivation-procedure
title: DEACTIVATION PROCEDURE
description: Details the deactivation procedure, traffic impact, execution steps,
  and verification commands for the Energy-Optimized Symbol Allocation feature.
tags:
- deactivation
- rancli
- energy-optimization
- symbol-allocation
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-30T17:16:12+00:00'
  source_sha256: 6bd343daccd55a01
sources:
- title: Energy-Optimized Symbol Allocation
  resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf
---

This section outlines the procedure to deactivate the Energy-Optimized Symbol Allocation feature, including traffic impact considerations, execution steps, and verification commands.

## Operational Impact and Behavior

- **Traffic impact**: None.
- **Timing**: Deactivation takes effect at the next scheduling occasion: all subsequent PDSCH assignments use the cell-default time-domain allocation.
- **In-flight processes**: In-flight HARQ processes complete with their original allocation shape.
- **UE and Cell status**: No UE reconfiguration or cell lock is involved.
- **Coexistence**: NR Micro Sleep Tx can remain active and will continue to mute naturally unoccupied symbols.

## Execution Steps

1. **Disable per cell**: Step 1 disables compaction per cell.
2. **Deactivate feature control**: Step 2 deactivates the feature control.
3. **Verify**: Step 3 verifies that `ctrCompactedAllocs` stops incrementing in the next ROP.

## CLI Commands

```text
# 1. Disable per cell 
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A symbolAllocMode=DISABLED 
 
# 2. Deactivate the feature 
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=EnergyOptSymbolAlloc featureState=DEACTIVATED 
 
# 3. Verify 
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A symbolAllocMode
```

# Cross-References

- [Activation Procedure](activation-procedure.md)
- [Parameters](parameters.md)
- [Performance Management](performance-management.md)
