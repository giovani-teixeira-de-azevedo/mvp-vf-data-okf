---
type: procedure
resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf#deactivation-procedure
title: DEACTIVATION PROCEDURE
description: Outlines the deactivation procedure, traffic impact, and CLI commands
  for CQI-Based UE Energy Efficiency Enhancement.
tags:
- deactivation
- rancli
- cqiEnergyEffMode
- FeatureCtrl
- CqiUeEnergyEff
- relaxedUeCount
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-25T17:21:23+00:00'
  source_sha256: a11b4e4481d89991
sources:
- title: CQI-Based UE Energy Efficiency Enhancement
  resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf
---

Traffic impact: none. On deactivation, new classifications stop immediately; UEs currently in the relaxed class are restored to the cell-default CSI configuration at their next RRC reconfiguration opportunity (within seconds), and the MCS bias is removed at once. Connected users notice nothing beyond a possible one-step MCS increase.

Step 1 disables the function per cell; step 2 deactivates the feature control node-wide; step 3 verifies that no UEs remain in the relaxed class after the reconfiguration sweep completes (allow up to one minute on a loaded cell). The license key may remain installed for later re-activation.

# 1. Disable per cell

```text
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A cqiEnergyEffMode=DISABLED
```

# 2. Deactivate the feature

```text
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=CqiUeEnergyEff featureState=DEACTIVATED
```

# 3. Verify no UEs remain relaxed

```text
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A relaxedUeCount
```

# Cross-References

* [ACTIVATION PROCEDURE](activation-procedure.md)
