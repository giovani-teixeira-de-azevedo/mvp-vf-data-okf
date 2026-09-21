---
type: concept
resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf#feature-operation
title: Feature Operation
description: Details candidate scoring, SCell selection, and dynamic rebalancing mechanisms
  for Dynamic Component Carrier Management.
tags:
- carrier-management
- scell
- carrier-aggregation
- scoring
- load-balancing
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:37:58+00:00'
  source_sha256: 6385d5da07ab0484
sources:
- resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf
  title: Dynamic Component Carrier Management
---

This section describes the operational mechanisms of the Dynamic Component Carrier Management feature, detailing how candidate SCells are evaluated, scored, selected, and dynamically rebalanced.

## Candidate Set Building and Scoring

At Secondary Cell (SCell) setup, the **Carrier Manager (CM)** builds a candidate set of component carriers. This set is constructed by taking the intersection of:
* The User Equipment (UE)'s reported band combinations (as per TS 38.306 capabilities).
* The node's configured carrier relations.

Each candidate carrier in the set is then evaluated using a multi-criteria scoring algorithm:

$$\text{score} = w_1 \cdot (1 - \text{prbLoad}) + w_2 \cdot \text{normRsrp} + w_3 \cdot \text{normUeThroughput}$$

Where the weights are configurable via the following parameters:
* $w_1$ is configured via `loadWeight`
* $w_2$ is configured via `rsrpWeight`
* $w_3$ is configured via `tputWeight`

## SCell Setup and Selection

Once candidate carriers are scored:
1. The Carrier Manager selects the top-scored carriers.
2. The top-scored carriers are configured as SCells via RRC Reconfiguration.
3. The number of configured SCells is capped up to the UE's maximum supported Component Carrier (CC) count.

## Periodic Re-evaluation and Dynamic SCell Swapping

To ensure optimal performance and load balancing, the Carrier Manager continuously evaluates active configurations:
* **Interval:** Every `reevalPeriod`, connected UEs with active SCells are re-scored.
* **Trigger Conditions:** An SCell swap is initiated if:
  1. A currently configured SCell's carrier exceeds a Physical Resource Block (PRB) load threshold defined by `rebalanceThr` for a duration of `rebalanceHoldTimer` seconds.
  2. An alternative candidate carrier scores at least `migrationMargin` points higher than the active SCell.
* **Execution:** When the trigger conditions are met, the Carrier Manager performs a paced SCell swap (removing carrier X and adding carrier Y) via RRC Reconfiguration.

## Operational Sequence Flow

The following sequence diagram illustrates the measurement, evaluation, selection, and periodic re-evaluation phases between the UE, Carrier Manager, and Scheduler:

```mermaid
sequenceDiagram 
    participant UE 
    participant CM as Carrier Manager 
    participant SCH as Scheduler 
    UE->>CM: Measurement report (A4) on candidate carriers 
    CM->>SCH: Query per-carrier load metrics 
    SCH-->>CM: PRB, PDCCH, active UE, throughput stats 
    CM->>CM: Score candidates, select top-N 
    CM->>UE: RRC Reconfiguration (SCell add, score order) 
    Note over CM: every reevalPeriod 
    CM->>CM: Re-score; carrier X > rebalanceThr 
    CM->>UE: RRC Reconfiguration (swap SCell X -> Y, paced)
```

# Cross-References

* [Feature Overview](feature-overview.md) — Overview of the Dynamic Component Carrier Management feature.
* [Parameters](parameters.md) — Detail of the configuration parameters (`loadWeight`, `rsrpWeight`, `tputWeight`, `reevalPeriod`, `rebalanceThr`, `rebalanceHoldTimer`, `migrationMargin`).
* [Activation Procedure](activation-procedure.md) — Sequence and steps required to activate the feature.
* [Deactivation Procedure](deactivation-procedure.md) — Steps and implications of disabling the feature.
