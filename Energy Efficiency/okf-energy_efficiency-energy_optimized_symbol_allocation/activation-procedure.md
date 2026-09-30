---
type: procedure
resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf#activation-procedure
title: Activation Procedure
description: Activation procedure, preconditions, traffic impact, and CLI commands
  for Energy-Optimized Symbol Allocation.
tags:
- energy-saving
- activation
- rancli
- symbol-allocation
- nr
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-30T17:16:16+00:00'
  source_sha256: 7907cbf45f1f5268
sources:
- title: Energy-Optimized Symbol Allocation
  resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf
---

This section describes the activation procedure for the Energy-Optimized Symbol Allocation feature, including preconditions, traffic impact, and step-by-step CLI commands.

## Traffic Impact and Preconditions

* **Traffic Impact:** None. Compaction decisions are made per assignment with a full-slot fallback always available, and every allocation is signaled through standard DCI — connected UEs follow it without reconfiguration. No cell lock or restart is required; the procedure can be executed during business hours.
* **Preconditions:**
  * License key `FAK-31540` installed.
  * NR Micro Sleep Tx activated on the same cells (otherwise savings are negligible).
  * Radio generation R2 or later for symbol-level muting.
* **Recommended Rollout:** Pilot cluster with defaults, verify Compaction BLER parity for one week, then extend network-wide.

## Activation Steps

The activation process consists of verifying the license, activating the feature control, enabling and tuning the feature per cell, and verifying the configuration. Post-change, confirm that the `ctrCompactedAllocs` counter increments under light traffic and that the Symbol Occupancy KPI steps down in off-peak ROPs.

### 1. Verify the license key is installed
```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=EnergyOptSymbolAlloc licenseState
# Expected: licenseState=ENABLED (key FAK-31540)
```

### 2. Activate the feature
```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=EnergyOptSymbolAlloc featureState=ACTIVATED
```

### 3. Enable and tune per cell
```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A symbolAllocMode=ENABLED
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A compactionLoadThr=30 minCompactedLength=4
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A maxCompactionUsers=4
```

### 4. Verify
```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A symbolAllocMode
```

# Cross-References

* [Feature Overview](feature-overview.md)
* [Feature Dependencies](feature-depedencies.md)
* [Feature Operation](feature-operation.md)
* [Network Impact](network-impact.md)
* [Parameters](parameters.md)
* [Performance Management](performance-management.md)
* [Deactivation Procedure](deactivation-procedure.md)
