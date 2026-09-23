---
type: procedure
resource: data/vodafone-mvp/raw/Data-Aware Carrier Management.pdf#activation-procedure
title: Activation Procedure
description: Details the activation procedure, preconditions, traffic impact, and
  CLI commands for Data-Aware Carrier Management.
tags:
- activation
- data-aware-carrier-management
- rancli
- procedure
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T13:52:49+00:00'
  source_sha256: a0ddcfc0ca37db02
sources:
- title: Data-Aware Carrier Management
  resource: data/vodafone-mvp/raw/Data-Aware Carrier Management.pdf
---

This section details the activation procedure for Data-Aware Carrier Management, including traffic impact, preconditions, rollout recommendations, and CLI commands.

## Overview, Preconditions, and Rollout

* **Traffic impact:** none. Activation changes only future SCell configuration decisions; existing UE configurations are untouched until their next natural reconfiguration. No cell lock or maintenance window is required.
* **Preconditions:** license key FAK-33012 installed, NR DL Carrier Aggregation active and carrying traffic, and a PM baseline collected.
* **Recommended rollout order:** activate on one representative node with defaults, verify after 48 hours that Promotion Latency P95 is under 60 ms and high-percentile burst throughput is unchanged, then roll out per cluster.
* **Procedure steps summary:** Step 1 verifies the license, step 2 activates the feature control, step 3 enables the function and sets the two headline thresholds explicitly, step 4 verifies.
* **Post-change:** confirm `ctrScellConfigSuppressed` increments while downlink throughput KPIs hold.

## Execution Steps

### 1. Verify the license key is installed
```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=DataAwareCarrierMgmt licenseState
# Expected: licenseState=ENABLED (key FAK-33012)
```

### 2. Activate the feature
```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=DataAwareCarrierMgmt featureState=ACTIVATED
```

### 3. Enable and tune
```bash
rancli set NodeRoot=1,NrFunction=1 dataAwareCmEnabled=true
rancli set NodeRoot=1,NrFunction=1 bulkBurstThr=500 interactiveBurstThr=50
```

### 4. Verify
```bash
rancli get NodeRoot=1,NrFunction=1 dataAwareCmEnabled
```

# Cross-References

* [Parameters](parameters.md)
* [Performance Management](performance-management.md)
* [Deactivation Procedure](deactivation-procedure.md)
