---
type: procedure
resource: data/vodafone-mvp/raw/Energy-Optimized Slot Allocation.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Outlines traffic impact, preconditions, rollout recommendations, procedure
  steps, and CLI commands for activating Energy-Optimized Slot Allocation.
tags:
- energy-saving
- slot-allocation
- activation
- rancli
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-30T09:42:23+00:00'
  source_sha256: f3bd6fe1007dbbe2
sources:
- resource: data/vodafone-mvp/raw/Energy-Optimized Slot Allocation.pdf
  title: Energy-Optimized Slot Allocation
---

This section describes the activation procedure for the Energy-Optimized Slot Allocation feature, including traffic impact, preconditions, recommended rollout, step-by-step procedure, and CLI execution commands.

## Overview and Preconditions

* **Traffic Impact**: Minor and bounded. Activation introduces up to `maxBatchDelay` milliseconds of additional queuing delay for non-exempt downlink traffic at low load. This is imperceptible to end users but is a real change to the latency profile; operators with strict latency SLAs on best-effort traffic should activate during a maintenance window and verify latency KPIs before the busy hour. No cell lock or restart is required.
* **Preconditions**: License key `FAK-31535` installed; NR Micro Sleep Tx should be activated first (or simultaneously) on the same cells so the created empty slots are monetized.
* **Recommended Rollout**: Pilot cluster with default parameters, verify Added Delay and throughput KPIs for one week, then extend.

## Activation Workflow

1. **Step 1**: Verify the license is installed.
2. **Step 2**: Activate the feature control.
3. **Step 3**: Enable per cell with the delay bound and fill target stated explicitly.
4. **Step 4**: Verify activation.

Post-change, confirm the Empty Slot Ratio steps up in the next off-peak ROPs and that P95 scheduling latency stays within the configured bound.

## CLI Execution Commands

```bash
# 1. Verify the license key is installed 
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=EnergyOptSlotAlloc licenseState 
# Expected: licenseState=ENABLED (key FAK-31535) 
 
# 2. Activate the feature 
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=EnergyOptSlotAlloc featureState=ACTIVATED 
 
# 3. Enable and tune per cell 
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A slotAllocMode=ENABLED 
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A maxBatchDelay=4 targetSlotFill=85 
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A batchingAllowedLoad=40 
 
# 4. Verify 
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A slotAllocMode
```

# Cross-References

* [FEATURE DEPEDENCIES](feature-depedencies.md)
* [PARAMETERS](parameters.md)
* [PERFORMANCE MANAGEMENT](performance-management.md)
* [DEACTIVATION PROCEDURE](deactivation-procedure.md)
