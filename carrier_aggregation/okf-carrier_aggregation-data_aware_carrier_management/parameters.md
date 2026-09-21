---
type: reference-table
resource: data/vodafone-mvp/raw/Data-Aware Carrier Management.pdf#parameters
title: PARAMETERS
description: Parameters and configuration scopes for Data-Aware Carrier Management.
tags:
- parameters
- configuration
- DACM
- FWA
- timers
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:37:38+00:00'
  source_sha256: 16c6db4c0eeb7a3c
sources:
- title: Data-Aware Carrier Management
  resource: data/vodafone-mvp/raw/Data-Aware Carrier Management.pdf
---

This section outlines the parameters, configuration scopes, and default values used to manage the Data-Aware Carrier Management (DACM) feature.

## Configuration Scope and Guidelines

Parameters are configured at the node level under `NrFunction=1`, with support for per-cell overrides of the burst thresholds.

* **Traffic Tuning:** Default parameter values are optimized for mixed smartphone traffic.
* **Fixed Wireless Access (FWA) Cells:** In cells dominated by FWA traffic, it is recommended to raise `bulkBurstThr` because almost all FWA traffic inherently qualifies as `BULK`.

## Parameters Table

| Parameter | Description | Values | Datatype | Default |
| :--- | :--- | :--- | :--- | :--- |
| `dataAwareCmEnabled` | Enables demand-based carrier management | `true`, `false` | `boolean` | `false` |
| `bulkBurstThr` | Burst size classifying a UE as BULK | 100–100000 (kB) | `int32` | 500 |
| `interactiveBurstThr` | Burst size classifying a UE as INTERACTIVE | 10–10000 (kB) | `int32` | 50 |
| `classDwellTimer` | Minimum time in a class before demotion | 100–10000 (ms) | `int32` | 1000 |
| `demoteTimer` | Idle time before SCell deactivation | 100–60000 (ms) | `int32` | 5000 |
| `deconfigTimer` | Further idle time before SCell de-configuration | 1000–300000 (ms) | `int32` | 30000 |
| `voiceUeMinClass` | Minimum class for UEs with 5QI-1 bearers | `INTERACTIVE`, `BULK` | `enum` | `INTERACTIVE` |
| `historyDepth` | Number of bursts kept in the per-UE history | 4–64 | `int32` | 16 |

# Cross-References

* [Feature Operation](feature-operation.md) — Describes the classification classes, timer operations, and SCell activation/deactivation logic governed by these parameters.
* [Activation Procedure](activation-procedure.md) — Details how to enable the `dataAwareCmEnabled` parameter and verify feature activation.
