---
type: procedure
resource: data/vodafone-mvp/raw/Fronthaul Sharing.pdf#deactivation-procedure
title: DEACTIVATION PROCEDURE
description: Step-by-step procedure to gracefully deactivate Fronthaul Sharing and
  decommission guest cells.
tags:
- fronthaul-sharing
- deactivation
- cell-lock
- decommissioning
- rancli
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:43:59+00:00'
  source_sha256: c46c919ff61b089b
sources:
- title: Fronthaul Sharing
  resource: data/vodafone-mvp/raw/Fronthaul Sharing.pdf
---

The **Deactivation Procedure** outlines the step-by-step sequence required to gracefully decommission guest cells and disable Fronthaul Sharing. 

## Traffic Impact & Guidelines

* **Traffic Impact:** All guest carriers on shared paths are taken out of service.
* **Maintenance Window:** Because this is a decommissioning operation for the guest's shared cells, it must run within a maintenance window agreed upon by both parties (both operators in sharing scenarios).
* **Order of Operations:** The sequence of steps is critical. You must lock and remove guest carriers first (Step 1) so that the sharing session terminates gracefully rather than via a path failure. Only then should you deactivate the guest role (Step 2) and release the host-side configuration (Step 3).
  
  > **Caution:** Deactivating the host-side first will trigger path-failure alarms and cause abrupt cell drops on the guest.
  
* **Verification:** Step 4 verifies that the host's own carriers remain unaffected.

---

## Step-by-Step Procedure

### Step 1: Lock Guest Cells (Guest)
Lock and remove the guest cells that are utilizing the shared path to ensure a graceful termination of the sharing session.

```bash
rancli lock NodeRoot=1,NrFunction=1,NrCell=N1A
rancli lock NodeRoot=1,NrFunction=1,NrCell=N1B
```

### Step 2: Deactivate Guest Role (Guest)
Disable the `FronthaulSharing` feature state on the guest node.

```bash
rancli set NodeRoot=1,EquipmentFunction=1,FeatureCtrl=FronthaulSharing featureState=DEACTIVATED
```

### Step 3: Release Sharing Configuration & Deactivate (Host)
On the host node, clear the configured shared ports and guest node ID, then deactivate the `FronthaulSharing` feature.

```bash
rancli set NodeRoot=1,EquipmentFunction=1,FronthaulSharing=1 sharedPortList="" guestNodeId=""
rancli set NodeRoot=1,EquipmentFunction=1,FeatureCtrl=FronthaulSharing featureState=DEACTIVATED
```

### Step 4: Verify Host Carriers (Host)
Verify that the host node's own carriers are unaffected by querying the session state.

```bash
rancli get NodeRoot=1,EquipmentFunction=1,FronthaulSharing=1 sessionState
```

# Cross-References

* [Activation Procedure](activation-procedure.md) — For the complementary procedure to activate Fronthaul Sharing.
* [Parameters](parameters.md) — For more information on `FronthaulSharing` attributes like `sharedPortList` and `guestNodeId`.
