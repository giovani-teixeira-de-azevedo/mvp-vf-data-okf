---
type: procedure
resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf#deactivation-procedure
title: Deactivation Procedure
description: Deactivation procedure for the Automated Energy Saver feature, including
  step-by-step CLI commands and impact details.
tags:
- deactivation
- cli-commands
- energy-saving
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:38:16+00:00'
  source_sha256: 7bfcd007cb2239e5
sources:
- resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf
  title: Automated Energy Saver
---

The deactivation procedure disables the automated control and returns threshold ownership to the subordinate features, ensuring a safe transition back to static configurations without impacting traffic.

## Impact & Behavior

- **Traffic Impact:** None. Connected users are unaffected, and no cell lock is required.
- **Deactivation Sequence:**
  1. The engine issues wake requests for every automated sleep state it currently owns.
  2. The engine returns threshold ownership back to the subordinate features, which resume operating on their own configured values.
- **Wake-up Times:** Wake-up completes within the standard wake-up times of the subordinate features:
  - **Micro/branch sleep:** Seconds.
  - **Radio deep sleep:** Up to 2 minutes.

> [!IMPORTANT]
> Review the subordinate features' statically configured thresholds before deactivating. If the node has run in AUTO mode for a long time, these statically configured thresholds might be stale.

---

## Step-by-Step Deactivation Procedure

### Step 1: Disable Automated Control
Disabling automated control triggers the ordered wake-and-release sequence.

```bash
rancli set NodeRoot=1,NrFunction=1 energySaverMode=OFF
```

### Step 2: Deactivate the Feature State
Deactivate the core control state of the `AutomatedEnergySaver` feature.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=AutomatedEnergySaver featureState=DEACTIVATED
```

### Step 3: Verify Subordinate Features Configuration
Verify that the subordinate features report and run on their own configured thresholds as active again.

```bash
rancli get NodeRoot=1,NrFunction=1 energySaverMode
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A sleepMode
```

# Cross-References

- [Activation Procedure](activation-procedure.md) — For details on activating the feature.
- [Feature Operation](feature-operation.md) — Information on how Automated Energy Saver interacts with subordinate features during runtime.
- [Network Impact](network-impact.md) — Evaluation of the impact of sleep modes on the network.
- [Parameters](parameters.md) — Description of the configuration parameters involved.
