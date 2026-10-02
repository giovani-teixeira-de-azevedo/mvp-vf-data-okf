---
type: concept
resource: data/vodafone-mvp/raw/NR Downlink Beam Optimizer.pdf#feature-operation
title: Feature Operation
description: Describes the evaluation cycle, scoring mechanism, open/closed-loop modes,
  and rollback behavior of the NR Downlink Beam Optimizer.
tags:
- beam-optimization
- open-loop
- closed-loop
- rollback
- evaluation-cycle
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T17:05:49+00:00'
  source_sha256: a1917e2ac0d5f8b0
sources:
- resource: data/vodafone-mvp/raw/NR Downlink Beam Optimizer.pdf
  title: NR Downlink Beam Optimizer
---

The NR Downlink Beam Optimizer operates on a periodic evaluation cycle to analyze traffic distribution and optimize beam configurations. It supports both manual (open-loop) and automated (closed-loop) execution modes, with built-in safeguards to prevent oscillation and performance degradation.

## Evaluation Cycle and Spatial Traffic Mapping

The optimizer runs an evaluation cycle per cell every `evalPeriod` hours. It aggregates per-beam counters and UE measurement reports collected since the last cycle to construct a **spatial traffic map**. 

For each beam, the spatial traffic map records:
*   **Access count**
*   **PDCP volume**
*   **RSRP percentiles** (P10, P50, and P90)
*   **Inter-beam switch matrix**

## Grid Scoring and Selection

Each candidate grid in the radio catalog is scored by predicting the traffic-weighted RSRP and the cell-edge coverage under that grid. This prediction is calculated using:
1.  The beam gain patterns.
2.  The observed angular traffic distribution.

To prevent oscillation between near-equivalent grids, a change is only triggered if the best candidate grid beats the current grid's score by at least `changeHysteresis` (default: 1.0 dB traffic-weighted RSRP equivalent).

## Execution Modes

The optimizer can be configured to run in one of two modes:

### Open-Loop Mode (`OPEN_LOOP`)
In open-loop mode, the optimizer acts as an advisory tool:
*   It generates a recommendation record in the Performance Management (PM) event stream.
*   It populates the `recommendedGrid` attribute, allowing the operator to review and manually apply the recommended grid.

### Closed-Loop Mode (`CLOSED_LOOP`)
In closed-loop mode, the optimizer automatically applies the grid during the next occurrence of the low-traffic window:
1.  **Reconfiguration**: Reconfigures connected-mode measurement objects.
2.  **Application**: Applies the new grid.
3.  **Verification**: Verifies SSB transmission on all new beams.
4.  **Guard Period & Rollback**: Monitors post-change accessibility over the next `guardPeriod`. If the accessibility drops by more than `rollbackThreshold` compared to the pre-change baseline, the optimizer automatically rolls back to the previous grid.

## Sequence Diagram

The following sequence diagram illustrates the closed-loop optimization process, including the evaluation, application, and rollback verification phases:

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

*   [Feature Overview](feature-overview.md) — For the high-level capabilities and architecture of the NR Downlink Beam Optimizer.
*   [Parameters](parameters.md) — For details on configuration parameters such as `evalPeriod`, `changeHysteresis`, `guardPeriod`, and `rollbackThreshold`.
*   [Performance Management](performance-management.md) — For details on the PM event stream and per-beam counters.
*   [Activation Procedure](activation-procedure.md) — For instructions on enabling the optimizer.
*   [Deactivation Procedure](deactivation-procedure.md) — For instructions on disabling the optimizer.
