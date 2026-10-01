---
type: procedure
resource: data/vodafone-mvp/raw/NR Mobility.pdf#deactivation-procedure
title: DEACTIVATION PROCEDURE
description: Step-by-step procedure to deactivate inter-frequency mobility and the
  NR Mobility feature.
tags:
- NR Mobility
- Deactivation
- RAN CLI
- Inter-frequency Mobility
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T09:19:19+00:00'
  source_sha256: 8bf2413875b8181f
sources:
- title: NR Mobility
  resource: data/vodafone-mvp/raw/NR Mobility.pdf
---

This section outlines the procedure to deactivate inter-frequency mobility and the NR Mobility feature. It includes traffic impact warnings, operational recommendations, and the specific RAN CLI commands required to disable and verify the deactivation.

## Traffic Impact and Operational Guidelines

Deactivating inter-frequency mobility is **service-affecting**. 

* **Coverage-Triggered Escape:** Deactivating inter-frequency mobility removes coverage-triggered escape to other layers. UEs leaving the serving layer's footprint will drop and recover via idle-mode reselection instead of performing a handover.
* **Scheduling:** Deactivation should only be performed as part of a deliberate layer-strategy change. A maintenance window should be scheduled if the layer carries significant traffic at its coverage edge.
* **Intra-Frequency Mobility:** Intra-frequency mobility is considered base functionality and is not deactivated in normal operation. If a cell needs to be taken out of service or isolated, it should be soft-locked (e.g., NR Cell Soft Lock) instead of deactivating intra-frequency mobility.
* **Post-Deactivation Monitoring:** Monitor the drop rate on the affected cells closely for the following day.

---

## Step-by-Step Deactivation Procedure

The deactivation process consists of three steps: disabling inter-frequency relations, deactivating the feature control, and verifying the status.

### Step 1: Disable Handover on the Inter-Frequency Relation
This step removes or disables the inter-frequency relations so that measurement configurations stop being issued to the UEs.

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A,NrFreqRelation=647328 connectedModeMobility=DISABLED
```

### Step 2: Deactivate the Feature
Deactivate the feature control for NR Mobility.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=NrMobility featureState=DEACTIVATED
```

### Step 3: Verify Deactivation
Verify that the feature state has been successfully updated to `DEACTIVATED`.

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=NrMobility featureState
```

# Cross-References

* [Activation Procedure](activation-procedure.md) - For details on enabling the NR Mobility feature.
* [Network Impact](network-impact.md) - For information on how mobility features affect network performance.
* [Feature Overview](feature-overview.md) - For general information on the NR Mobility feature.
