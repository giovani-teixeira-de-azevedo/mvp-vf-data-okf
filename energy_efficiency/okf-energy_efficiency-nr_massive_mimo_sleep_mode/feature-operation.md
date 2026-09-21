---
type: concept
resource: data/vodafone-mvp/raw/NR Massive MIMO Sleep Mode.pdf#feature-operation
title: Feature Operation
description: Describes the operational behavior, load threshold evaluation, and signaling
  sequence of the NR Massive MIMO Sleep Mode feature.
tags:
- Massive MIMO
- Sleep Mode
- gNodeB Scheduler
- Power Saving
- Load Evaluation
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:42:04+00:00'
  source_sha256: 2bfda528665467b9
sources:
- title: NR Massive MIMO Sleep Mode
  resource: data/vodafone-mvp/raw/NR Massive MIMO Sleep Mode.pdf
---

The **NR Massive MIMO Sleep Mode** feature is controlled on a per-NR-cell basis. It uses continuous evaluation of cell load to dynamically transition the AAS Radio Unit (RU) into a power-saving state (sleep level 1) while preserving basic coverage.

## Dynamic Load Evaluation and Sleep Entry

The gNodeB scheduler continuously evaluates two load measures over a sliding window:
*   **Average downlink PRB utilization**
*   **Number of RRC-connected UEs**

When both measures fall below their respective entry thresholds (`prbLoadEnterThr` and `connUsersEnterThr`) and remain below them for the duration of the entry timer (`sleepEnterTimer` seconds), the cell initiates the sleep entry process. 

Upon entering sleep mode, the cell:
1.  Suspends MU-MIMO pairing.
2.  Requests the AAS Radio Unit to gate the configured branch subset (disabling the branches and turning off Power Amplifier (PA) bias).
3.  Switches the SSB beam configuration to a pre-computed reduced-branch grid to ensure coverage of the broadcast channels is preserved.

Once active in sleep mode, the cell continues to serve traffic on the remaining active branches.

## Sleep Exit

When the load exceeds the exit threshold, the gNodeB scheduler triggers the exit process:
1.  The cell control requests the AAS Radio Unit to restore the full branch configuration.
2.  The AAS Radio Unit activates all branches. This restoration takes less than 2 seconds (`< 2 s`).

## Time-of-Day Restriction

An optional time-of-day window can be configured via parameters `sleepAllowedStart` and `sleepAllowedStop`. This window restricts when the cell is allowed to enter sleep mode. This configuration is recommended for cells with highly volatile night-time load, such as event venues, to prevent unnecessary sleep transitions.

## Sequence of Operations

The sequence of interactions between the gNodeB Scheduler (SCH), Cell Control (CC), and AAS Radio Unit (RU) during sleep entry and exit is illustrated below:

```mermaid
sequenceDiagram 
    participant SCH as gNodeB Scheduler 
    participant CC as Cell Control 
    participant RU as AAS Radio Unit 
    SCH->>CC: Load below thresholds (timer expired) 
    CC->>SCH: Suspend MU-MIMO pairing 
    CC->>RU: Branch gating request (sleep level 1) 
    RU-->>CC: Branch subset disabled, PA bias off 
    CC->>SCH: Apply reduced beam grid 
    Note over SCH,RU: Cell serves traffic on active branches 
    SCH->>CC: Load above exit threshold 
    CC->>RU: Restore full branch configuration 
    RU-->>CC: All branches active (< 2 s)
```

# Cross-References

*   [Feature Overview](feature-overview.md) — For a high-level description of the NR Massive MIMO Sleep Mode feature.
*   [Parameters](parameters.md) — For descriptions of thresholds and timers such as `prbLoadEnterThr`, `connUsersEnterThr`, and `sleepEnterTimer`.
*   [Performance Management](performance-management.md) — For monitoring the impact of sleep mode operations.
*   [Activation Procedure](activation-procedure.md) — For instructions on enabling this feature.
