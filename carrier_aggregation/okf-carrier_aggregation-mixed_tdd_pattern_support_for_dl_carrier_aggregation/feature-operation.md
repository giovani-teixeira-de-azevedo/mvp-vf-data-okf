---
type: concept
resource: data/vodafone-mvp/raw/Mixed TDD Pattern Support for DL Carrier Aggregation.pdf#feature-operation
title: Feature Operation
description: Configuration and runtime operation of Mixed TDD Pattern Support for
  DL Carrier Aggregation, including HARQ-ACK multiplexing and feedback offloading.
tags:
- tdd
- carrier-aggregation
- harq-ack
- pucch
- scheduling
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:38:57+00:00'
  source_sha256: 7dcd598b913ed91b
sources:
- resource: data/vodafone-mvp/raw/Mixed TDD Pattern Support for DL Carrier Aggregation.pdf
  title: Mixed TDD Pattern Support for DL Carrier Aggregation
---

This section describes the configuration and runtime operation of the Mixed TDD Pattern Support for DL Carrier Aggregation feature. It covers carrier aggregation compatibility checks, HARQ-ACK feedback scheduling, and feedback offloading mechanisms.

## Carrier Aggregation (CA) Configuration

During Carrier Aggregation configuration, the combination builder performs compatibility checks and maps the feedback paths:
*   **Pattern Compatibility:** The system checks pattern compatibility, specifically verifying the periodicity ratio and checking for conflict slots in the case of half-duplex UEs.
*   **Per-CC $k_1$ Set Computation:** It computes a per-Component Carrier (CC) $k_1$ mapping set. This ensures that every downlink slot across all configured CCs maps to a valid Primary Cell (PCell) uplink occasion within the UE's HARQ capability window.

## Runtime Operation and Scheduling

During active operation, scheduling and feedback multiplexing proceed as follows:
*   **Independent Scheduling:** The per-CC schedulers run independently on their native TDD patterns.
*   **Dynamic HARQ-ACK Codebook:** The Type-2 (dynamic) HARQ-ACK codebook is enabled. This dynamic codebook allows feedback for a variable number of CC/slot combinations to pack efficiently into each PUCCH occasion.
*   **Capacity Reservation:** The PCell PUCCH scheduler reserves capacity based on the worst-case multiplexed codebook size to ensure feedback reliability.

### Operation Sequence

The sequence of downlink assignment, feedback bridging, and multiplexing is illustrated below:

```mermaid
sequenceDiagram 
    participant SCH as Scheduler (per CC) 
    participant UE 
    participant PC as PCell PUCCH 
    SCH->>UE: DL assignment on SCell slot n (SCell pattern) 
    SCH->>UE: DL assignment on PCell slot n (PCell pattern) 
    Note over UE: k1 set bridges pattern offset 
    UE->>PC: HARQ-ACK multiplexed (Type-2 codebook) in next PCell UL slot 
    PC-->>SCH: Feedback demultiplexed per CC 
    SCH->>UE: Retransmissions per CC as needed
```

## PUCCH Feedback Offloading

If the PUCCH load on the PCell approaches saturation, the feature can offload feedback to reduce the signaling load on the PCell:
*   **Secondary PUCCH Group:** Feedback can be offloaded to a same-pattern SCell PUCCH group.
*   **Configuration Parameter:** This is enabled by setting `pucchGroupMode` to `DUAL`.
*   **Impact:** Offloading to a dual PUCCH group halves the PCell PUCCH signaling burden.

# Cross-References

* [Feature Overview](feature-overview.md)
* [Parameters](parameters.md)
