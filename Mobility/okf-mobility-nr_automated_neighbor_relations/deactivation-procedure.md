---
type: procedure
resource: data/vodafone-mvp/raw/NR Automated Neighbor Relations.pdf#deactivation-procedure
title: Deactivation Procedure
description: Step-by-step procedure to disable the Automated Neighbor Relations (ANR)
  function and deactivate the feature.
tags:
- ANR
- Deactivation
- RAN CLI
- gNodeB
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T16:54:03+00:00'
  source_sha256: cb79f3e9d3050534
sources:
- title: NR Automated Neighbor Relations
  resource: data/vodafone-mvp/raw/NR Automated Neighbor Relations.pdf
---

This section describes the procedure to deactivate the Automated Neighbor Relations (ANR) feature on the gNodeB. It outlines the traffic impact, operational considerations, and the specific RAN CLI commands required to disable the function, deactivate the feature control, and verify that existing relations are retained.

### Impact and Considerations

*   **Traffic Impact:** None immediately. However, neighbor data begins to age.
*   **Functional Impact:** Deactivation stops neighbor discovery, automatic Xn setup, and automatic removal.
*   **Relation Table:** The existing relation table is retained and continues to serve handovers.
*   **Long-term Impact:** Over time, missing new neighbors (such as new sites or re-parented cells) will degrade mobility. Therefore, deactivation should be temporary (e.g., during a controlled neighbor audit) rather than permanent.
*   **Requirements:** No cell lock and no maintenance window are needed.

---

### Step-by-Step Deactivation

The deactivation procedure consists of three steps: freezing the function, deactivating the feature control, and verifying that the relation table remains intact.

#### Step 1: Disable the ANR Function
Freeze the ANR function by setting the `anrState` parameter to `DISABLED`.

```bash
rancli set NodeRoot=1,NrFunction=1 anrState=DISABLED
```

#### Step 2: Deactivate the Feature
Deactivate the feature control by setting the `featureState` parameter to `DEACTIVATED`.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=NrAnr featureState=DEACTIVATED
```

#### Step 3: Verify Relations are Retained
Verify that the existing relation table is intact by checking the neighbor relation count.

```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A neighborRelationCount
```

# Cross-References

*   [Activation Procedure](activation-procedure.md) — For details on how to enable and activate the ANR feature.
*   [Network Impact](network-impact.md) — For details on how ANR affects network performance and traffic.
*   [Parameters](parameters.md) — For details on the configuration parameters used in these commands.
