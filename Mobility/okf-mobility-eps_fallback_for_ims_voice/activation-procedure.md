---
type: procedure
resource: data/vodafone-mvp/raw/EPS Fallback for IMS Voice.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Step-by-step procedure to activate and configure EPS Fallback for IMS
  Voice.
tags:
- EPS Fallback
- IMS Voice
- Activation
- Configuration
- CLI
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T11:07:25+00:00'
  source_sha256: 8f79bf5a69410c73
sources:
- title: EPS Fallback for IMS Voice
  resource: data/vodafone-mvp/raw/EPS Fallback for IMS Voice.pdf
---

This section describes the step-by-step activation and configuration procedure for the EPS Fallback for IMS Voice feature. It outlines the traffic impact, preconditions, recommended rollout strategy, and CLI commands required to enable and verify the feature.

## Traffic Impact & Preconditions

*   **Traffic Impact:** None. Activation only changes how future 5QI 1 flow requests are handled; ongoing sessions and data traffic are unaffected. The procedure can be executed during business hours, though the first voice attempts after activation should be verified promptly.
*   **Preconditions:**
    *   License key `FAK-33121` installed.
    *   LTE target carriers identified and VoLTE-verified for every NR cell's coverage area.
    *   For the `HANDOVER` method, N26 interface confirmed operational with the core network team and inter-RAT neighbor relations populated (verify NR Automated Neighbor Relations is running).
    *   The restart must be done only if it is Sunday.

## Recommended Rollout Order

1.  Begin with `fallbackMethod=REDIRECT` and measurements enabled on a pilot cluster.
2.  Verify the KPIs and end-to-end test calls.
3.  Migrate mature clusters to `HANDOVER_WITH_REDIRECT_FALLBACK`.

## Step-by-Step Activation Procedure

### Step 1: Verify the license key is installed
Verify that the license key `FAK-33121` is installed and enabled.

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=EpsFallbackImsVoice licenseState
```

**Expected Output:**
```text
licenseState=ENABLED (key FAK-33121)
```

### Step 2: Activate the feature
Activate the feature control state.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=EpsFallbackImsVoice featureState=ACTIVATED
```

### Step 3: Configure per-cell policy
Configure the per-cell policy, target list, and thresholds explicitly for auditability.

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A voiceFallbackMode=FALLBACK_ALWAYS
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A fallbackMethod=REDIRECT fallbackMeasEnabled=true
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A lteTargetFreqList=1300,6300 b1ThresholdRsrp=-118
```

### Step 4: Verify configuration
Verify that the cell-level parameters are correctly applied.

```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A voiceFallbackMode fallbackMethod
```

## Post-Change Verification

*   Place test calls from SA-camped UEs at cell center and cell edge.
*   Confirm that the counter `ctrEpsFbSuccess` increments and the calls establish as VoLTE.
*   Confirm that the Fallback Success Rate is greater than 99% over the first full day.

# Cross-References

*   [Feature Dependencies](feature-depedencies.md)
*   [Parameters](parameters.md)
*   [Performance Management](performance-management.md)
*   [Deactivation Procedure](deactivation-procedure.md)
