---
type: concept
resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf#feature-operation
title: FEATURE OPERATION
description: Describes the operational sequence, candidate scoring formula, and periodic
  re-evaluation rules for SCell selection and swapping in Dynamic Component Carrier
  Management.
tags:
- carrier-management
- scell
- rrc-reconfiguration
- scoring
- load-balancing
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T15:05:03+00:00'
  source_sha256: 6385d5da07ab0484
sources:
- title: Dynamic Component Carrier Management
  resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf
---

This section describes the operational mechanisms of Dynamic Component Carrier Management, including SCell candidate selection, scoring, periodic re-evaluation, and SCell swapping procedures.

## Candidate Set Building and Scoring

At SCell setup, the carrier manager constructs a candidate set by taking the intersection of the UE's reported band combinations (TS 38.306 capabilities) and the node's carrier relations.

Each candidate carrier is evaluated using the following scoring equation:

$$\text{score} = w_1 \cdot (1 - \text{prbLoad}) + w_2 \cdot \text{normRsrp} + w_3 \cdot \text{normUeThroughput}$$

The weighting factors are configurable via parameters:
- `loadWeight` ($w_1$)
- `rsrpWeight` ($w_2$)
- `tputWeight` ($w_3$)

Carriers with the highest scores are configured as SCells, up to the maximum CC count supported by the UE.

## Periodic Re-evaluation and SCell Swapping

Every `reevalPeriod`, connected UEs with active SCells undergo re-scoring. SCell swapping via RRC Reconfiguration is triggered under the following conditions:
- A configured SCell's carrier exceeds `rebalanceThr` PRB load for `rebalanceHoldTimer` seconds.
- An alternative carrier scores at least `migrationMargin` points higher than the current SCell.

## Operation Sequence

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

- [FEATURE OVERVIEW](feature-overview.md)
- [PARAMETERS](parameters.md)
