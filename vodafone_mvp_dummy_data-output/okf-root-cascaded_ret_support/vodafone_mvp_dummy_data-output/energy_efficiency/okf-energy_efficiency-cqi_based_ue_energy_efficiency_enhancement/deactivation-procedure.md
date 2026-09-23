---
type: procedure
resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf#deactivation-procedure
title: DEACTIVATION PROCEDURE
description: Step-by-step procedure and CLI commands for deactivating the CQI-Based
  UE Energy Efficiency Enhancement feature.
tags:
- cqi
- ue-energy-efficiency
- deactivation
- rancli
- procedure
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T14:17:45+00:00'
  source_sha256: a11b4e4481d89991
sources:
- title: CQI-Based UE Energy Efﬁciency Enhancement
  resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf
---

This section outlines the deactivation procedure and traffic impact when disabling the CQI-Based UE Energy Efficiency Enhancement feature.

## Traffic Impact

Deactivation has no traffic impact. Upon deactivation:
- New classifications stop immediately.
- UEs currently in the relaxed class are restored to the cell-default CSI configuration at their next RRC reconfiguration opportunity (within seconds).
- The MCS bias is removed at once.
- Connected users notice nothing beyond a possible one-step MCS increase.

## Deactivation Steps

Step 1 disables the function per cell; step 2 deactivates the feature control node-wide; step 3 verifies that no UEs remain in the relaxed class after the reconfiguration sweep completes (allow up to one minute on a loaded cell). The license key may remain installed for later re-activation.

### 1. Disable per cell

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A cqiEnergyEffMode=DISABLED
```

### 2. Deactivate the feature

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=CqiUeEnergyEff featureState=DEACTIVATED
```

### 3. Verify no UEs remain relaxed

```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A relaxedUeCount
```

# Cross-References

- [ACTIVATION PROCEDURE](activation-procedure.md)
- [PARAMETERS](parameters.md)
