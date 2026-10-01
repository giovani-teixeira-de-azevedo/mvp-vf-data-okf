---
type: procedure
resource: data/vodafone-mvp/raw/NR Mobility.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Step-by-step procedure to verify licenses, activate inter-frequency mobility,
  and configure mobility thresholds.
tags:
- activation
- mobility
- configuration
- rancli
- nr
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T09:19:25+00:00'
  source_sha256: 375843490ed405e2
sources:
- resource: data/vodafone-mvp/raw/NR Mobility.pdf
  title: NR Mobility
---

This section describes the activation procedure for the NR Mobility inter-frequency extension. While intra-frequency mobility is active by default from cell unlock, inter-frequency mobility requires license verification, feature activation, and threshold configuration.

## Traffic Impact & Preconditions

*   **Traffic Impact:** None. Intra-frequency mobility is part of the base package and active from cell unlock. Activating the licensed inter-frequency extension arms additional measurement configuration for future connections only; existing connections pick it up at their next reconfiguration. Business-hours activation is safe.
*   **Preconditions:**
    *   License key `FAK-33100` installed for inter-frequency mobility (see [Feature Dependencies](feature-depedencies.md)).
    *   Frequency relations created toward each candidate layer with priorities agreed in the network's layering strategy.
    *   Neighbor relations present (verify NR Automated Neighbor Relations is active, or audit manual relations).
*   **Recommended Rollout:** Enable inter-frequency mobility layer pair by layer pair, starting where coverage-triggered A5 is genuinely needed, and hold A2/A1 thresholds conservative until gap cost is quantified on live traffic.

## Step-by-Step Procedure

The activation consists of four main steps:
1.  Verify the license.
2.  Activate the feature control.
3.  Create and configure the frequency relation with explicit thresholds for auditability.
4.  Verify the configuration.

```bash
# 1. Verify the license key is installed 
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=NrMobility licenseState 
# Expected: licenseState=ENABLED (key FAK-33100) 
 
# 2. Activate the feature 
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=NrMobility featureState=ACTIVATED 
 
# 3. Configure mobility thresholds per cell and frequency relation 
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A a3Offset=3 hysteresisA3=2 timeToTriggerA3=320 
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A a2SearchThr=-106 a1StopThr=-100 
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A,NrFreqRelation=647328 a5Threshold1Rsrp=-112 a5Threshold2Rsrp=-106 
 
# 4. Verify 
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A a3Offset a2SearchThr
```

## Post-Change Verification

*   Confirm A2-triggered measurement configuration in UE traces.
*   Watch `ctrHoExecOk` on the new relation (see [Performance Management](performance-management.md)).
*   Review the MRO counters after 48 hours before any threshold tuning.

# Cross-References

*   [Feature Dependencies](feature-depedencies.md)
*   [Parameters](parameters.md)
*   [Performance Management](performance-management.md)
*   [Deactivation Procedure](deactivation-procedure.md)
