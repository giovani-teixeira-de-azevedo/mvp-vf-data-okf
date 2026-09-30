---
type: procedure
resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf#deactivation-procedure
title: DEACTIVATION PROCEDURE
description: Deactivation procedure for Automated Energy Saver, including wake-and-release
  behavior and rancli commands.
tags:
- deactivation
- energy-saver
- rancli
- configuration
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-29T16:08:45+00:00'
  source_sha256: 7bfcd007cb2239e5
sources:
- resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf
  title: Automated Energy Saver
---

This document details the deactivation procedure for the Automated Energy Saver feature on a node using `rancli` commands, including the execution steps and verification requirements.

## Deactivation Behavior and Impact

- **Traffic Impact:** None. Connected users are unaffected and no cell lock is required.
- **Wake-and-Release Sequence:** Upon deactivation, the engine first issues wake requests for every automated sleep state it owns. It then returns threshold ownership to the subordinate features, which resume operating on their own configured values.
- **Wake-up Duration:** Wake-up completes within the wake-up times of the subordinate features (seconds for micro/branch sleep, up to 2 minutes for radio deep sleep).
- **Prerequisite Review:** Statically configured thresholds of subordinate features should be reviewed prior to deactivation, as they may be stale if the node ran in `AUTO` for a long duration.

## Procedure Steps

### 1. Disable Automated Control
Moves the node to `OFF`, triggering the ordered wake-and-release sequence:

```bash
rancli set NodeRoot=1,NrFunction=1 energySaverMode=OFF
```

### 2. Deactivate the Feature
Deactivates feature control:

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=AutomatedEnergySaver featureState=DEACTIVATED
```

### 3. Verify Configuration
Verifies that subordinate features report their own configured thresholds as active again:

```bash
rancli get NodeRoot=1,NrFunction=1 energySaverMode
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A sleepMode
```
