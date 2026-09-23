---
type: procedure
resource: data/vodafone-mvp/raw/Data-Aware Carrier Management.pdf#deactivation-procedure
title: DEACTIVATION PROCEDURE
description: Outlines the traffic impact and steps for node deactivation and reverting
  to static CA behavior.
tags:
- deactivation
- rancli
- carrier-aggregation
- nr-function
- feature-control
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T13:53:07+00:00'
  source_sha256: 67637336b1965349
sources:
- title: Data-Aware Carrier Management
  resource: data/vodafone-mvp/raw/Data-Aware Carrier Management.pdf
---

This section outlines the deactivation procedure and traffic impact when disabling node function decisions and feature control.

Traffic impact: none. On deactivation the node reverts to static CA behavior: CA-capable UEs are again configured with all candidate SCells at their next reconfiguration. Connected UEs are not force-reconfigured, so the transition happens gradually and transparently.

Step 1 disables the function so the estimator stops influencing decisions; step 2 deactivates the feature control; step 3 verifies. Expect RRC reconfiguration volume and SCell activation counts to return to baseline within an hour as the connected-UE population turns over.

## 1. Disable the function

```text
rancli set NodeRoot=1,NrFunction=1 dataAwareCmEnabled=false
```

## 2. Deactivate the feature

```text
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=DataAwareCarrierMgmt featureState=DEACTIVATED
```

## 3. Verify

```text
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=DataAwareCarrierMgmt featureState
```

# Cross-References

- [ACTIVATION PROCEDURE](activation-procedure.md)
