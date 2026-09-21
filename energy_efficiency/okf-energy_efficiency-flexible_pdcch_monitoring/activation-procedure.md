---
type: procedure
resource: data/vodafone-mvp/raw/Flexible PDCCH Monitoring.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Step-by-step procedure to verify licensing, configure pilot cell parameters,
  and activate Flexible PDCCH Monitoring.
tags:
- activation
- configuration
- rancli
- pdcch
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:41:36+00:00'
  source_sha256: 7c5ef5a708e47fda
sources:
- title: Flexible PDCCH Monitoring
  resource: data/vodafone-mvp/raw/Flexible PDCCH Monitoring.pdf
---

This section outlines the step-by-step activation procedure for the Flexible PDCCH Monitoring feature, including preconditions, rollout recommendations, CLI commands, and post-change verification.

## Procedural Context & Impact

* **Traffic Impact:** None. The feature reconfigures capable UEs through standard RRC procedures as they connect or are reconfigured. Existing connections adopt the configuration at their next reconfiguration.
* **Signaling:** Monitoring adaptation is layer-1 signaled and reversible within slots.
* **Execution Window:** The procedure can be safely executed during business hours.

## Preconditions

Prior to execution, ensure the following conditions are met:
1. **License Key:** License key `FAK-31545` must be installed.
2. **DRX Configuration:** Connected Mode DRX must be active with an inactivity timer longer than the planned `sssgSwitchTimer`. For details, see [Feature Dependencies](feature-depedencies.md).

## Recommended Rollout Strategy

1. **Phase 1 (Pilot):** Start with `pdcchMonitoringMode=SSSG_ONLY` (switching only, no skipping) on a pilot cluster.
2. **Phase 2 (Evaluation):** Verify the Switch Rate and latency KPIs for one week. For more on KPIs, see [Performance Management](performance-management.md).
3. **Phase 3 (Full Activation):** After pilot review, enable `SSSG_AND_SKIP`.

---

## Step-by-Step Activation

### Step 1: Verify the License Key Installation
Verify that the required license key `FAK-31545` is installed and enabled.

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=FlexPdcchMonitoring licenseState
```
* **Expected Output:** `licenseState=ENABLED` (key `FAK-31545`)

### Step 2: Activate the Feature Control
Activate the feature at the function level.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=FlexPdcchMonitoring featureState=ACTIVATED
```

### Step 3: Pilot Configuration (Group Switching Only)
Enable the pilot mode with the timer and period stated explicitly on the cell (e.g., cell `N1A`).

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A pdcchMonitoringMode=SSSG_ONLY
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A sparsePeriod=4 sssgSwitchTimer=8
```

### Step 4: Upgrade to Skipping (Post-Pilot Review)
Once pilot performance is verified, upgrade the configuration to allow skipping by enabling `SSSG_AND_SKIP` and setting `maxSkipDuration` to 20.

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A pdcchMonitoringMode=SSSG_AND_SKIP maxSkipDuration=20
```

### Step 5: Verify the Configuration
Confirm that the cell is operating in the intended PDCCH monitoring mode.

```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A pdcchMonitoringMode
```

---

## Post-Change Verification

After executing the changes, monitor the following indicators over the subsequent hours:
* **Sparse Monitoring Share:** Confirm that this share rises as UEs reconnect over the following hours.
* **Latency Impact:** Verify that the `ctrLateFirstPacket` counter remains proportionate.

# Cross-References

* [Feature Dependencies](feature-depedencies.md) — For prerequisites and interaction with Connected Mode DRX timers.
* [Parameters](parameters.md) — For detailed descriptions of `pdcchMonitoringMode`, `sparsePeriod`, `sssgSwitchTimer`, and `maxSkipDuration`.
* [Performance Management](performance-management.md) — For details on tracking KPIs like the Sparse Monitoring Share and `ctrLateFirstPacket`.
* [Deactivation Procedure](deactivation-procedure.md) — For steps to roll back or deactivate this feature.
