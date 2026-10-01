---
type: procedure
resource: data/vodafone-mvp/raw/NR Automated Neighbor Relations.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Step-by-step procedure to activate and configure the NR Automated Neighbor
  Relations (ANR) feature.
tags:
- ANR
- Activation
- Configuration
- RAN CLI
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T16:54:00+00:00'
  source_sha256: 2573c6caa73a50b7
sources:
- title: NR Automated Neighbor Relations
  resource: data/vodafone-mvp/raw/NR Automated Neighbor Relations.pdf
---

This section outlines the step-by-step procedure for activating and configuring the NR Automated Neighbor Relations (ANR) feature. It includes preconditions, traffic impact considerations, recommended rollout strategies, CLI commands, and post-change verification steps.

## Traffic Impact & Scheduling

* **Traffic Impact:** Negligible. CGI reading introduces autonomous gaps for individual reporting UEs, which are bounded well below 0.5% of cell throughput by the per-cell order cap (`maxCgiOrdersPerCell`).
* **Maintenance Window:** None required. Activation during business hours is normal practice, as daytime traffic actually accelerates neighbor discovery.

## Preconditions

Before initiating the activation procedure, ensure the following requirements are met:
1. **License Key:** License key `FAK-33130` must be installed.
2. **Core Network Support:** AMF support for NG Configuration Transfer must be confirmed with the core network team.
3. **Security/Firewall:** Firewall and IPsec policies for Xn SCTP must be verified if `autoXnSetup` is used.
4. **Relation Review:** Existing manually created relations must be reviewed, and critical relations must be marked as `noRemove=true` before enabling automatic removal.

## Recommended Rollout Order

* Activate with `anrState=ALL` and `autoXnSetup=true` network-wide (ANR is safe to deploy broadly).
* Hold `interRatAnrEnabled=false` until LTE target frequencies are configured on the NR cells.

---

## Step-by-Step Activation Procedure

### Step 1: Verify the License Key
Verify that the required license key is installed and enabled on the node.

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=NrAnr licenseState
```
* **Expected Output:** `licenseState=ENABLED` (associated with key `FAK-33130`)

### Step 2: Activate the Feature Control
Enable the feature state for NR ANR.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=NrAnr featureState=ACTIVATED
```

### Step 3: Configure the ANR Function
Set the function-level parameters, including the ANR state, automatic Xn setup, relation removal timer, and CGI order limits.

```bash
rancli set NodeRoot=1,NrFunction=1 anrState=ALL autoXnSetup=true
rancli set NodeRoot=1,NrFunction=1 relationRemovalTime=30 maxCgiOrdersPerCell=4
```

### Step 4: Protect Critical Relations
Protect critical manually planned relations from automatic removal by setting the `noRemove` attribute to `true`.

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A,NrCellRelation=N1B noRemove=true
```

### Step 5: Verify Configuration
Confirm that the ANR state has been successfully applied.

```bash
rancli get NodeRoot=1,NrFunction=1 anrState
```

---

## Post-Change Verification

Within 24 to 48 hours of activation, perform the following checks:
* Confirm that the counter `ctrRelationCreated` shows growth consistent with the cell's environment.
* Verify that the CGI Resolution Success rate is above 90%.

# Cross-References

* [Feature Dependencies](feature-depedencies.md) — Details the prerequisites and core network dependencies.
* [Parameters](parameters.md) — Explains the parameters configured during this procedure (e.g., `anrState`, `autoXnSetup`, `relationRemovalTime`, `maxCgiOrdersPerCell`).
* [Performance Management](performance-management.md) — Describes the counters used for post-change verification (e.g., `ctrRelationCreated`).
* [Deactivation Procedure](deactivation-procedure.md) — Steps to safely disable the ANR feature.
