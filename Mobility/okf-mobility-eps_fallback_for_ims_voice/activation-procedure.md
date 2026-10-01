---
type: procedure
resource: data/vodafone-mvp/raw/EPS Fallback for IMS Voice.pdf#activation-procedure
title: Activation Procedure
description: Step-by-step procedure to activate and configure EPS Fallback for IMS
  Voice.
tags:
- activation
- configuration
- EPS Fallback
- RAN CLI
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T10:33:36+00:00'
  source_sha256: 8f79bf5a69410c73
sources:
- title: EPS Fallback for IMS Voice
  resource: data/vodafone-mvp/raw/EPS Fallback for IMS Voice.pdf
---

This section outlines the step-by-step activation and configuration procedure for the EPS Fallback for IMS Voice feature. It includes traffic impact analysis, preconditions, recommended rollout strategies, CLI commands, and post-change verification steps.

## Traffic Impact
* **Impact:** None.
* **Details:** Activation only changes how future 5QI 1 flow requests are handled. Ongoing sessions and data traffic are unaffected.
* **Scheduling:** The procedure can be executed during business hours, though the first voice attempts after activation should be verified promptly.

## Preconditions
Before executing the activation procedure, ensure the following requirements are met:
* License key `FAK-33121` is installed (see [Feature Dependencies](feature-depedencies.md)).
* LTE target carriers are identified and VoLTE-verified for every NR cell's coverage area.
* For the `HANDOVER` method, the N26 interface must be confirmed operational with the core network team, and inter-RAT neighbor relations must be populated (verify that NR Automated Neighbor Relations is running).

## Recommended Rollout Order
1. Begin with `fallbackMethod=REDIRECT` and measurements enabled (`fallbackMeasEnabled=true`) on a pilot cluster.
2. Verify the KPIs and end-to-end test calls.
3. Migrate mature clusters to `HANDOVER_WITH_REDIRECT_FALLBACK`.

## Step-by-Step Activation Procedure

### Step 1: Verify the License Key
Verify that the required license key is installed and enabled.
```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=EpsFallbackImsVoice licenseState
```
* **Expected Output:** `licenseState=ENABLED` (key `FAK-33121`)

### Step 2: Activate the Feature
Activate the feature control state.
```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=EpsFallbackImsVoice featureState=SUPER_ACTIVATED
```

### Step 3: Configure Per-Cell Policy
Configure the per-cell policy, target list, and thresholds explicitly for auditability. The following example configures cell `N1A` (for parameter details, see [Parameters](parameters.md)):
```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A voiceFallbackMode=FALLBACK_ALWAYS
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A fallbackMethod=REDIRECT fallbackMeasEnabled=true
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A lteTargetFreqList=1300,6300 b1ThresholdRsrp=-118
```

### Step 4: Verify Configuration
Verify that the cell-level parameters have been applied correctly.
```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A voiceFallbackMode fallbackMethod
```

## Post-Change Verification
Perform the following verification steps immediately after activation:
1. Place test calls from Standalone (SA) camped UEs at both the cell center and cell edge.
2. Confirm that the `ctrEpsFbSuccess` counter increments (see [Performance Management](performance-management.md)) and that the calls establish successfully as VoLTE.
3. Confirm that the Fallback Success Rate is greater than 99% over the first full day of operation.

# Cross-References
* [Feature Dependencies](feature-depedencies.md)
* [Parameters](parameters.md)
* [Performance Management](performance-management.md)
* [Deactivation Procedure](deactivation-procedure.md)
