---
type: procedure
resource: data/vodafone-mvp/raw/Fronthaul Sharing.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Step-by-step procedure to activate the Fronthaul Sharing feature on host
  and guest nodes, including licensing verification, configuration commands, and session
  validation.
tags:
- activation
- procedure
- fronthaul-sharing
- configuration
- cli
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:43:45+00:00'
  source_sha256: 7e68fa380603ce4d
sources:
- resource: data/vodafone-mvp/raw/Fronthaul Sharing.pdf
  title: Fronthaul Sharing
---

This section describes the step-by-step procedure for activating the Fronthaul Sharing feature, including traffic impact considerations, configuration commands for host and guest roles, and post-activation validation.

## Traffic Impact

* **Host Cells:** None.
* **Guest Cells:** Since guest cells are created onto the shared path, this is treated as an integration activity rather than a change to live traffic.
* **Migrating Existing Guest Carriers:** If migrating existing guest carriers from dedicated fiber onto a shared path, those carriers will incur an outage of typically 5–15 minutes. This migration must be executed within a maintenance window.

## Preconditions

Prior to starting the activation, ensure that the following requirements are met:
1. **Licensing:** License key `FAK-51040` must be installed on both host and guest nodes.
2. **Connectivity:** IP reachability between host and guest must be established for the coordination session.
3. **Synchronization:** Phase synchronization must be verified on both nodes if TDD carriers will be shared.
4. **Optical Path:** The optical budget must be validated for the aggregate rate.

### Recommended Configuration Sequence
1. Fully configure the host side (Steps 1–3).
2. Configure the guest side (Step 4).
3. Verify the coordination session and path delay (Step 5) before unlocking any guest carrier.

---

## Step-by-Step Activation

### Step 1: (Host) Verify the license key is installed

Query the license state of the feature on the host node to confirm that key `FAK-51040` is ready.

```bash
rancli get NodeRoot=1,EquipmentFunction=1,FeatureCtrl=FronthaulSharing licenseState
```

* **Expected Output:** `licenseState=ENABLED` (key `FAK-51040`)

### Step 2: (Host) Activate and set role

Activate the feature state and set the `sharingRole` parameter to `HOST` on the host node.

```bash
rancli set NodeRoot=1,EquipmentFunction=1,FeatureCtrl=FronthaulSharing featureState=ACTIVATED
rancli set NodeRoot=1,EquipmentFunction=1,FronthaulSharing=1 sharingRole=HOST
```

### Step 3: (Host) Declare shared ports, guest and reservation

Configure the list of shared ports, specify the guest node identity, and assign the bandwidth reservation on the host node.

```bash
rancli set NodeRoot=1,EquipmentFunction=1,FronthaulSharing=1 sharedPortList=FH-3,FH-4 guestNodeId=GNB-4402 guestMaxBandwidth=10
```

### Step 4: (Guest) Activate and reference the host

On the guest node, activate the feature, configure the guest role, and point to the host node identity.

```bash
rancli set NodeRoot=1,EquipmentFunction=1,FeatureCtrl=FronthaulSharing featureState=ACTIVATED
rancli set NodeRoot=1,EquipmentFunction=1,FronthaulSharing=1 sharingRole=GUEST hostNodeId=ENB-1201
```

### Step 5: Verify session and delay

Query the status of the coordination session and check the measured path delay.

```bash
rancli get NodeRoot=1,EquipmentFunction=1,FronthaulSharing=1 sessionState measuredPathDelay
```

* **Expected Output:** `sessionState=ESTABLISHED`, `measuredPathDelay` < `maxSharedPathDelay`

---

## Post-Activation Monitoring

Following activation, monitor performance counters to ensure the health of the connection.
* Watch the `ctrKeepAliveLoss` counter for **24 hours**.
* On a healthy segment, this counter should remain at **zero**.

# Cross-References

* [Deactivation Procedure](deactivation-procedure.md) — For steps to disable or revert the configuration.
* [Feature Dependencies](feature-depedencies.md) — For prerequisites and functional dependencies.
* [Parameters](parameters.md) — Detailed description of parameter values like `maxSharedPathDelay`.
* [Performance Management](performance-management.md) — Details on performance counters including `ctrKeepAliveLoss`.
