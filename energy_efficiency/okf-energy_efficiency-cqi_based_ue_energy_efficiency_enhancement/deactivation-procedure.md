---
type: procedure
resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf#deactivation-procedure
title: Deactivation Procedure
description: Step-by-step procedure to disable and deactivate the CQI-Based UE Energy
  Efficiency Enhancement feature.
tags:
- deactivation
- rancli
- configuration
- energy-efficiency
- cqi
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:39:01+00:00'
  source_sha256: a11b4e4481d89991
sources:
- title: CQI-Based UE Energy Efficiency Enhancement
  resource: data/vodafone-mvp/raw/CQI-Based UE Energy Efficiency Enhancement.pdf
---

This section describes the procedure to disable and deactivate the CQI-Based UE Energy Efficiency Enhancement feature, detailing the traffic impact, system behavior, and required command-line operations.

## Traffic Impact and Deactivation Behavior

*   **Traffic Impact:** None.
*   **Deactivation Effects:**
    *   New UE classifications stop immediately.
    *   The MCS bias is removed at once.
    *   Connected users experience no service disruption, with only a potential one-step MCS increase noticeable.
*   **CSI Configuration Restoration:** UEs currently in the relaxed class are restored to the cell-default CSI configuration at their next RRC reconfiguration opportunity (typically within seconds).
*   **License Key:** The license key does not need to be uninstalled and may remain installed for later re-activation.

---

## Step-by-Step Deactivation

### Step 1: Disable Per Cell
Disable the energy efficiency mode on the target cell (e.g., `N1A`).

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A cqiEnergyEffMode=DISABLED
```

### Step 2: Deactivate the Feature Control
Deactivate the feature control node-wide.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=CqiUeEnergyEff featureState=DEACTIVATED
```

### Step 3: Verify Deactivation
Verify that no UEs remain in the relaxed class. Allow up to one minute on a loaded cell for the RRC reconfiguration sweep to complete.

```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A relaxedUeCount
```

# Cross-References

*   [Activation Procedure](activation-procedure.md) - The procedure to enable and activate this feature.
*   [Parameters](parameters.md) - Parameter definitions, including `cqiEnergyEffMode`, `featureState`, and `relaxedUeCount`.
