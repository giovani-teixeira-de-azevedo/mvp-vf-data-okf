---
type: concept
resource: data/vodafone-mvp/raw/NR Massive MIMO Sleep Mode.pdf#feature-operation
title: Feature Operation
description: Details the operation of NR Massive MIMO Sleep Mode, including cell load
  evaluation, sequence transitions, and time-of-day restrictions.
tags:
- massive-mimo
- sleep-mode
- gnodeb
- energy-saving
- nr-cell
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-25T16:09:21+00:00'
  source_sha256: 2bfda528665467b9
sources:
- title: NR Massive MIMO Sleep Mode
  resource: data/vodafone-mvp/raw/NR Massive MIMO Sleep Mode.pdf
---

This section details the operational mechanics of the NR Massive MIMO Sleep Mode feature, including load evaluation, beam grid switching, branch gating sequences, and time-window restrictions.

## Load Evaluation and Branch Gating

The feature is controlled per NR cell. The gNodeB scheduler continuously evaluates two load measures over a sliding window:
- Average downlink PRB utilization
- Number of RRC-connected UEs

When both measures fall below `prbLoadEnterThr` and `connUsersEnterThr` for `sleepEnterTimer` seconds, the cell requests the radio to gate the configured branch subset. The SSB beam configuration is switched to a pre-computed reduced-branch grid so that coverage of the broadcast channels is preserved.

## Transition Sequence

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

## Time-of-Day Window

An optional time-of-day window (`sleepAllowedStart` / `sleepAllowedStop`) restricts when sleep may be entered, which is recommended for cells with volatile night-time load such as event venues.

# Cross-References

- [Feature Overview](feature-overview.md)
- [Parameters](parameters.md)
