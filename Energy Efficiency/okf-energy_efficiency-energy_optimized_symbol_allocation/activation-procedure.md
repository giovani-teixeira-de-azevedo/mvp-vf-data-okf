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
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-29T22:48:29+00:00'
  source_sha256: 7907cbf45f1f5268
sources:
- resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf
  title: Energy-Optimized Symbol Allocation
---

Traffic impact: none. Compaction decisions are made per assignment with a full-slot fallback always available, and every allocation is signaled through standard DCI — connected UEs follow it without reconfiguration. No cell lock or restart is required; the procedure can be executed during business hours.

Preconditions: license key FAK-31540 installed; NR Micro Sleep Tx activated on the same cells (otherwise savings are negligible); radio generation R2 or later for symbol-level muting. Recommended rollout: pilot cluster with defaults, verify Compaction BLER parity for one week, then extend network-wide.

Step 1 verifies the license; step 2 activates the feature control; step 3 enables per cell with explicit guard values; step 4 verifies. Post-change, confirm `ctrCompactedAllocs` increments under light traffic and that the Symbol Occupancy KPI steps down in off-peak ROPs.

## Activation Steps

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

- [Feature Dependencies](feature-depedencies.md)
- [Parameters](parameters.md)
- [Performance Management](performance-management.md)
- [Deactivation Procedure](deactivation-procedure.md)
