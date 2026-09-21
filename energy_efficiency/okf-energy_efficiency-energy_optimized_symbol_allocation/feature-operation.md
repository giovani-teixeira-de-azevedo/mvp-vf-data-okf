---
type: concept
resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf#feature-operation
title: Feature Operation
description: Detailed operational workflow and scheduler decision logic of the Energy-Optimized
  Symbol Allocation feature, including symbol compaction and PA muting.
tags:
- energy-saving
- scheduler
- symbol-compaction
- downlink
- pa-muting
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:41:06+00:00'
  source_sha256: 70ee2b28bbf85295
sources:
- resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf
  title: Energy-Optimized Symbol Allocation
---

This section describes the operational logic of the Energy-Optimized Symbol Allocation feature. It outlines the decision-making process in the scheduler for symbol compaction and the signaling interaction between the scheduler, radio, and user equipment (UE).

## Operational Mechanism

When the cell load is below the threshold defined by the parameter `compactionLoadThr` (detailed in [Parameters](parameters.md)), the scheduler evaluates candidate downlink assignments to determine if symbol compaction can be applied. For each candidate downlink assignment, the scheduler calculates two potential allocations:

1. **Default Full-Slot Allocation**: The standard time-frequency resource allocation.
2. **Compacted Allocation**: An allocation with the same Transport Block Size (TBS) but configured with the maximum available PRB (Physical Resource Block) width and the shortest Start and Length Indicator Value (SLIV) that can carry the block at the selected Modulation and Coding Scheme (MCS).

### Selection Criteria

The scheduler selects the compacted variant if the following conditions are met:
* The compacted variant fits within the available frequency resources.
* The estimated Signal-to-Interference-plus-Noise Ratio (SINR) supports the wider PRB allocation (wider allocations average over more frequency-selective fading, which is typically a wash).

### Radio and Transmit Execution

Once the compacted allocation is chosen:
* **Symbol Map Forwarding**: The scheduler forwards the per-slot symbol occupation map to the Radio Unit (RU) one slot in advance.
* **PA Muting**: The radio mutes its Power Amplifier (PA) starting from the last occupied symbol until the end of the slot, thereby saving transmit energy.
* **UE Allocation**: The scheduler sends the Downlink Control Information (DCI) to the UE indicating the shortened time-domain allocation (e.g., allocation length).

## Process Flow

The sequence below illustrates the interaction between the scheduler, the internal time-domain allocator, the radio unit, and the UE during an active compacted downlink transmission:

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

## HARQ and Uplink Handling

* **HARQ Retransmissions**: To maintain soft-combining alignment, HARQ retransmissions reuse the original allocation shape.
* **Uplink**: The uplink is unaffected by this feature. Symbol compaction is strictly a downlink transmit-energy-saving mechanism.

# Cross-References

* [Feature Overview](feature-overview.md) — For a high-level description of the Energy-Optimized Symbol Allocation feature.
* [Parameters](parameters.md) — For information on the configuration parameter `compactionLoadThr`.
* [Network Impact](network-impact.md) — For how this symbol compaction affects performance KPIs.
