---
type: procedure
resource: data/vodafone-mvp/raw/Long Fronthaul over eCPRI.pdf#deactivation-procedure
title: Deactivation Procedure
description: Deactivation procedure for the Long Fronthaul over eCPRI feature, including
  port-level disablement and feature deactivation commands.
tags:
- deactivation
- ecpri
- long-fronthaul
- rancli
- configuration
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:45:16+00:00'
  source_sha256: 91dfbdae0b7e959e
sources:
- resource: data/vodafone-mvp/raw/Long Fronthaul over eCPRI.pdf
  title: Long Fronthaul over eCPRI
---

This document outlines the procedure to deactivate the Long Fronthaul over eCPRI feature. It includes the steps to disable the mode per port, deactivate the feature globally, and verify the network state, along with important traffic impact and prerequisite considerations.

## Traffic Impact & Pre-requisites

> [!WARNING]
> **Traffic impact:** Cells on the affected port are restarted once, causing an outage of approximately 3–10 minutes. 
> 
> The feature must **only** be deactivated if the physical fronthaul distance is within the standard range. Deactivating the feature on a genuinely long path will take the cells permanently out of service. A maintenance window must be planned.

This procedure applies when:
- Radios are being re-homed to a nearby baseband (C-RAN consolidation reversal).
- A path has been physically shortened.

### Deactivation Behavior
- **Step 1** disables long-fronthaul mode per port, which reverts HARQ timing to standard budgets and restarts the cells. If the measured delay exceeds the standard budget, the cells stay barred with a delay-budget alarm (expected and correct behavior).
- **Step 2** deactivates the feature control once all ports are reverted.
- **Step 3** verifies the configuration and cell operational status.

---

## Step-by-Step Procedure

### Step 1: Disable Per Fronthaul Port
Disable the long-fronthaul mode on the target port. This will restart the cells on that port and requires the path to be within the standard range.

```bash
rancli set NodeRoot=1,EquipmentFunction=1,FronthaulPort=FH-1 longFronthaulMode=DISABLED
```

### Step 2: Deactivate the Feature
Once all ports have been reverted to standard mode, deactivate the overall feature control.

```bash
rancli set NodeRoot=1,EquipmentFunction=1,FeatureCtrl=LongFronthaulEcpri featureState=DEACTIVATED
```

### Step 3: Verify Configuration and Status
Verify that the `longFronthaulMode` is disabled and check the operational state of the cells.

```bash
rancli get NodeRoot=1,EquipmentFunction=1,FronthaulPort=FH-1 longFronthaulMode
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A operationalState
```

# Cross-References
* [Activation Procedure](activation-procedure.md)
* [Network Impact](network-impact.md)
* [Parameters](parameters.md)
* [Feature Overview](feature-overview.md)
