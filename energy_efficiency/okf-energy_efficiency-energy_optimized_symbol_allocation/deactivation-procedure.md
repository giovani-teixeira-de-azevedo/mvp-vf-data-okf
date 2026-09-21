---
type: procedure
resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf#deactivation-procedure
title: Deactivation Procedure
description: Step-by-step procedure to deactivate the Energy-Optimized Symbol Allocation
  feature, including commands and verification.
tags:
- RAN
- Energy Saving
- Deactivation
- CLI
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:41:12+00:00'
  source_sha256: 6bd343daccd55a01
sources:
- resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf
  title: Energy-Optimized Symbol Allocation
---

This section outlines the step-by-step procedure to deactivate the Energy-Optimized Symbol Allocation feature, including operational impacts and verification commands.

## Traffic and Operational Impact

* **Traffic Impact:** None.
* **Scheduling:** Deactivation takes effect at the next scheduling occasion. All subsequent PDSCH (Physical Downlink Shared Channel) assignments use the cell-default time-domain allocation.
* **In-flight HARQ:** In-flight HARQ processes complete with their original allocation shape.
* **Disruption:** No UE reconfiguration or cell lock is involved.
* **Coexistence:** NR Micro Sleep Tx can remain active and will continue to mute naturally unoccupied symbols.

## Deactivation Procedure

### Step 1: Disable Per Cell
Disable compaction for the target cell (e.g., cell ID `N1A`):

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A symbolAllocMode=DISABLED
```

### Step 2: Deactivate the Feature Control
Deactivate the overall feature state:

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=EnergyOptSymbolAlloc featureState=DEACTIVATED
```

### Step 3: Verify Deactivation
Verify that the allocation mode has been set to `DISABLED`:

```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A symbolAllocMode
```

Additionally, verify that the performance counter `ctrCompactedAllocs` stops incrementing in the next Recording Observation Period (ROP).

# Cross-References

* [Activation Procedure](activation-procedure.md) — Steps for activating and enabling the feature.
* [Parameters](parameters.md) — Details on parameters like `symbolAllocMode`.
* [Performance Management](performance-management.md) — Details on performance counters such as `ctrCompactedAllocs`.
