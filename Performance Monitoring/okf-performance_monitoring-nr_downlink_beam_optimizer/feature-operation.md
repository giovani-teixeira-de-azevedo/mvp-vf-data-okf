---
type: concept
resource: data/vodafone-mvp/raw/NR Downlink Beam Optimizer.pdf#feature-operation
title: Feature Operation
description: Describes the evaluation cycle, scoring mechanism, open-loop and closed-loop
  modes, and rollback behavior of the NR Downlink Beam Optimizer.
tags:
- NR
- Downlink Beam Optimizer
- Open Loop
- Closed Loop
- Beam Optimization
- Rollback
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T14:12:54+00:00'
  source_sha256: a1917e2ac0d5f8b0
sources:
- title: NR Downlink Beam Optimizer
  resource: data/vodafone-mvp/raw/NR Downlink Beam Optimizer.pdf
---

This section describes the operational behavior of the NR Downlink Beam Optimizer, detailing its evaluation cycle, scoring mechanism, open-loop and closed-loop modes, and automatic rollback procedures.

## Evaluation Cycle and Scoring

The optimizer runs an evaluation cycle per cell every `evalPeriod` hours. During each cycle, it aggregates the per-beam counters and UE measurement reports collected since the last cycle into a spatial traffic map. This map contains the following per-beam metrics:
*   Access count
*   PDCP volume
*   RSRP percentiles (P10/P50/P90)
*   Inter-beam switch matrix

Each candidate grid in the radio catalog is scored by predicting the traffic-weighted RSRP and the cell-edge coverage under that grid. This prediction uses the beam gain patterns and the observed angular traffic distribution. 

To trigger a change and prevent oscillation between near-equivalent grids, the best candidate grid must beat the current grid's score by at least `changeHysteresis` (default 1.0 dB traffic-weighted RSRP equivalent).

## Operational Modes

The NR Downlink Beam Optimizer can operate in one of two modes:

### Open Loop Mode (`OPEN_LOOP`)
In `OPEN_LOOP` mode, the optimizer does not apply changes automatically. Instead, it generates:
*   A recommendation record in the PM event stream.
*   A `recommendedGrid` attribute for the operator to review and apply manually.

### Closed Loop Mode (`CLOSED_LOOP`)
In `CLOSED_LOOP` mode, the optimizer automatically applies the selected grid during the next occurrence of the low-traffic window. The automated process consists of the following steps:
1.  **Reconfiguration:** Reconfigures connected-mode measurement objects.
2.  **Application:** Applies the new grid.
3.  **Verification:** Verifies SSB transmission on all new beams.
4.  **Guard Period & Rollback:** Monitors post-change accessibility over the next `guardPeriod`. If the accessibility drops by more than the `rollbackThreshold` compared to the pre-change baseline, the optimizer automatically rolls back to the previous grid.

## Sequence Diagram

The following sequence diagram illustrates the closed-loop optimization process:

```mermaid
sequenceDiagram 
    participant OPT as Optimizer 
    participant CC as Cell Control 
    participant RU as AAS Radio 
    participant PM as PM Subsystem 
    OPT->>PM: Read per-beam stats (evalPeriod) 
    OPT->>OPT: Score catalog grids vs current 
    OPT->>CC: Apply grid G7 (low-traffic window) 
    CC->>RU: Reconfigure SSB beam set 
    RU-->>CC: Grid active (< 1 s gap) 
    CC->>PM: Baseline accessibility watch (guardPeriod) 
    alt accessibility drop > rollbackThreshold 
        OPT->>CC: Roll back to previous grid 
    end
```

# Cross-References

*   [Feature Overview](feature-overview.md) — For an overview of the NR Downlink Beam Optimizer capabilities.
*   [Parameters](parameters.md) — For details on configuration parameters such as `evalPeriod`, `changeHysteresis`, `guardPeriod`, and `rollbackThreshold`.
*   [Performance Management](performance-management.md) — For details on PM counters and event streams.
