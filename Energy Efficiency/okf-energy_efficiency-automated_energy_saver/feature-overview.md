---
type: concept
resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf#feature-overview
title: Feature Overview
description: High-level overview of the Automated Energy Saver feature for orchestrating
  gNodeB energy-saving functions.
tags:
- automated-energy-saver
- energy-saving
- gnodeb
- orchestration
- traffic-prediction
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-29T16:08:03+00:00'
  source_sha256: 532828ecd10dd38e
sources:
- resource: data/vodafone-mvp/raw/Automated Energy Saver.pdf
  title: Automated Energy Saver
---

This section provides a high-level overview of the Automated Energy Saver feature, which serves as a node-level orchestration layer for individual energy-saving functions within the gNodeB.

## Overview and Problem Statement

Automated Energy Saver provides node-level orchestration for gNodeB energy-saving functions. Instead of requiring manual tuning of thresholds and time windows for each cell, the feature builds a per-cell traffic prediction model to arm, tune, and sequence underlying energy savers without operator intervention. The coordinated features include:

- NR Booster Carrier Sleep
- NR Massive MIMO Sleep Mode
- Radio Deep Sleep Mode
- NR Micro Sleep Tx

The primary issue addressed is feature coordination. Individual energy features operate safely in isolation, but their thresholds interact:
- A booster carrier sleeping too early shifts traffic load to the coverage carrier, preventing it from reaching its sleep-entry threshold.
- Partial sleep in a Massive MIMO cell alters PRB utilization figures relied upon by radio deep-sleep decisions.

## Decision Mechanism and Hierarchy

Automated Energy Saver manages the decision hierarchy by:
1. Learning cell diurnal load patterns from PM counter history using a 21-day rolling window with separate weekday and weekend profiles.
2. Forecasting load for the upcoming decision interval.
3. Computing target energy states per cell and per radio.

Individual features execute state transitions through standard mechanisms, preserving built-in safeguards such as emergency-call blocking and wake-up guarantees.

```mermaid
flowchart TD 
    PM[PM counter history<br>21-day rolling window] --> MDL[Traffic prediction model] 
    MDL --> DEC[Energy state decision engine] 
    CFG[Operator policy<br>savingsLevel, protected hours] --> DEC 
    DEC --> BCS[NR Booster Carrier Sleep] 
    DEC --> MMS[NR Massive MIMO Sleep Mode] 
    DEC --> RDS[Radio Deep Sleep Mode] 
    BCS --> RU[Radio units] 
    MMS --> RU 
    RDS --> RU 
    RU --> PM
```

## Operator Policy and Performance

Operators maintain control via a concise set of parameters:
- `savingsLevel`: Overall savings ambition level.
- Protected hours: Time windows during which automated actions are suspended.
- Per-cell opt-out: Ability to exclude specific cells from automation.

In field deployments, automated coordination typically achieves an additional 5–15% site energy saving compared to statically configured features, primarily by extending sleep windows into shoulder hours left unaddressed by static time-of-day settings.

# Cross-References

- [Parameters](parameters.md)
- [Feature Operation](feature-operation.md)
