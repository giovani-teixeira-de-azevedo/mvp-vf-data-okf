---
type: procedure
resource: data/vodafone-mvp/raw/Energy-Optimized Power Allocation.pdf#activation-procedure
title: Activation Procedure
description: Step-by-step activation and tuning procedure for the Energy-Optimized
  Power Allocation feature.
tags:
- RAN
- Activation
- Tuning
- Configuration
- Commissioning
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:39:23+00:00'
  source_sha256: be6de7a37ed90adc
sources:
- title: Energy-Optimized Power Allocation
  resource: data/vodafone-mvp/raw/Energy-Optimized Power Allocation.pdf
---

This section describes the step-by-step configuration and verification procedure for activating the Energy-Optimized Power Allocation feature, including pre-requisites and rollout recommendations.

## Impact & Preconditions

### Traffic & Network Impact
*   **Traffic Impact:** None. The feature only ever reduces power on links with verified headroom, one allocation at a time, and the closed-loop guard restores full power on any sign of degradation.
*   **Service Availability:** No cell lock or restart is needed; the activation procedure can be executed during business hours. For detailed analysis, see [Network Impact](network-impact.md).

### Preconditions
*   **License Key:** License key `FAK-31530` must be installed.
*   **Feature Interactions:** If *EMF Power Lock Mid-Band* or *Modulation-Aware Power Control* is active on the target cells, confirm their configuration first so the combined power behavior is understood (refer to [Feature Dependencies](feature-depedencies.md)).

### Recommended Rollout Strategy
1.  Activate on a pilot cluster at the default margin.
2.  Verify BLER and throughput KPIs at baseline for one week.
3.  Extend network-wide.
4.  Tighten `powerMargin` only per-cluster and 1 dB at a time (refer to [Parameters](parameters.md) for details on parameter configuration).

---

## Step-by-Step Execution

### Step 1: Verify the License Key Installation
Confirm that the license key is installed and active on the node.
```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=EnergyOptPowerAlloc licenseState
```
**Expected Output:**
```text
licenseState=ENABLED (key FAK-31530)
```

### Step 2: Activate the Feature Control
Enable the feature at the Node/NF function level.
```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=EnergyOptPowerAlloc featureState=ACTIVATED
```

### Step 3: Enable and Tune Per Cell
Enable the feature on specific cells (e.g., `N1A`) with explicit margin and power reduction configurations for auditability.
```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A energyPowerAllocMode=ENABLED
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A powerMargin=3 maxPowerReduction=4
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A paBiasTracking=ON
```

### Step 4: Verify Cell-Level Configuration
Verify that the cell-level energy power allocation mode has successfully switched to `ENABLED`.
```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A energyPowerAllocMode
```

---

## Post-Change Verification

After executing the configuration steps, monitor performance management counters to ensure proper operation:
*   Confirm that the `ctrReducedAllocs` counter increments within minutes under traffic.
*   Ensure that the ratio of guard events to reduced allocations stays below the acceptable threshold:
    $$\frac{\text{ctrGuardEvents}}{\text{ctrReducedAllocs}} < 2\%$$

For more details on performance counter monitoring, see [Performance Management](performance-management.md).

---

# Cross-References

*   [Feature Dependencies](feature-depedencies.md) — Information on interactions with EMF Power Lock Mid-Band and Modulation-Aware Power Control.
*   [Parameters](parameters.md) — Detailed explanation of parameters like `powerMargin`, `maxPowerReduction`, and `paBiasTracking`.
*   [Network Impact](network-impact.md) — Overview of traffic and performance impact.
*   [Performance Management](performance-management.md) — Monitoring counters including `ctrReducedAllocs` and `ctrGuardEvents`.
*   [Deactivation Procedure](deactivation-procedure.md) — Instructions to disable the feature if needed.
