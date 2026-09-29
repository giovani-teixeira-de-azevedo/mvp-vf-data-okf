---
type: concept
resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf#feature-operation
title: FEATURE OPERATION
description: Describes the operational mechanism of energy-optimized symbol compaction
  in downlink scheduling and radio unit power amplifier muting.
tags:
- energy-saving
- symbol-allocation
- downlink
- scheduler
- pa-muting
- sliv
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-29T22:41:36+00:00'
  source_sha256: 70ee2b28bbf85295
sources:
- title: Energy-Optimized Symbol Allocation
  resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf
---

This section describes the operational mechanics of the Energy-Optimized Symbol Allocation feature, detailing how the scheduler computes compacted allocations, instructs the radio unit to mute power amplifiers, and handles HARQ retransmissions.

## Allocation and Compaction Logic

For each candidate downlink assignment when system load is below `compactionLoadThr` (see [Parameters](parameters.md)), the scheduler evaluates two candidate allocations:

1. **Default Full-Slot Allocation**: Standard allocation spanning the default symbol duration (e.g., 13 symbols).
2. **Compacted Allocation**: An allocation with the same Transport Block Size (TBS) using the maximum available PRB width and the shortest Start and Length Indicator Value (SLIV) that carries the block at the selected Modulation and Coding Scheme (MCS).

### Selection Criteria
The scheduler selects the compacted variant if:
- The compacted allocation fits within the available frequency resources.
- The estimated SINR supports the wider allocation (noting that wider allocations average over more frequency-selective fading, which is usually a wash).

## Radio Unit (RU) PA Muting

- The per-slot symbol occupation map is forwarded to the radio unit **one slot ahead**.
- The radio unit mutes the Power Amplifier (PA) starting from the last occupied symbol until the end of the slot.

## Operation Sequence

```mermaid
sequenceDiagram 
    participant SCH as Scheduler 
    participant TD as Time-Domain Allocator 
    participant RU as Radio Unit 
    participant UE as UE 
    SCH->>TD: Assignment: TBS 12 kB, MCS 18, load 12% 
    TD->>TD: Default: 13 symbols × 18 PRB 
    TD->>TD: Compacted: 6 symbols × 40 PRB (same TBS) 
    TD->>SCH: Choose compacted (SLIV start 1, length 6) 
    SCH->>UE: DCI: time-domain allocation length 6 
    SCH->>RU: Symbol map: symbols 7–13 unoccupied 
    RU->>RU: PA muted symbols 7–13 
    UE-->>SCH: HARQ ACK
```

## Special Considerations

- **HARQ Retransmissions**: HARQ retransmissions reuse the original allocation shape to maintain soft-combining alignment.
- **Uplink Impact**: Uplink is unaffected; symbol compaction is exclusively a downlink transmit-energy feature.

# Cross-References

- [Parameters](parameters.md)
