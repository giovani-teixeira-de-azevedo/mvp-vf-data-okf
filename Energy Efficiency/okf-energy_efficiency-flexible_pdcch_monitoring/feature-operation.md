---
type: concept
resource: data/vodafone-mvp/raw/Flexible PDCCH Monitoring.pdf#feature-operation
title: FEATURE OPERATION
description: Describes search space set group (SSSG) switching, PDCCH skipping, and
  per-5QI monitoring profiles in Flexible PDCCH Monitoring.
tags:
- pdcch
- sssg
- sssg0
- sssg1
- 5qi
- scheduler
- nr
- rrc
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-30T14:19:54+00:00'
  source_sha256: 5ddced3f4c7d975c
sources:
- title: Flexible PDCCH Monitoring
  resource: data/vodafone-mvp/raw/Flexible PDCCH Monitoring.pdf
---

This section describes the operational mechanics of Flexible PDCCH Monitoring within the gNodeB scheduler, including Search Space Set Group (SSSG) switching, PDCCH skipping, and per-5QI monitoring profiles.

## SSSG Switching and PDCCH Skipping

At RRC connection setup or reconfiguration, capable UEs are configured with two search space set groups:
- **SSSG0**: Dense monitoring with cell default periodicity.
- **SSSG1**: Sparse monitoring every `sparsePeriod` slots.

The scheduler tracks per-UE activity:
- While data or HARQ feedback is pending, the UE is kept in SSSG0.
- When `sssgSwitchTimer` expires after the last scheduled activity, the gNodeB indicates a switch to SSSG1.
- New data arrival triggers an immediate switch back — the first assignment is sent in an SSSG1 occasion together with the group-switch indication, avoiding extra round-trip time.
- When the scheduler positively knows a quiet window (for example, after the final segment of a VoNR talk spurt with silence descriptor pacing, or after an RRC release has been decided but not yet executed), it issues a skip indication covering that window.

## Interaction Sequence

```mermaid
sequenceDiagram 
    participant UE as UE (Rel-17) 
    participant SCH as gNodeB Scheduler 
    SCH->>UE: RRC: SSSG0 (every slot), SSSG1 (every 4 slots) 
    SCH->>UE: Data burst on SSSG0 occasions 
    Note over SCH: Last HARQ ACK received, timer starts 
    SCH->>UE: DCI: switch to SSSG1 
    UE->>UE: Monitor 1 of 4 slots (Rx energy down) 
    SCH->>UE: New data: assignment in SSSG1 occasion + switch to SSSG0 
    SCH->>UE: DCI: skip PDCCH 20 ms (known quiet window) 
    UE->>UE: Receiver off between DRX wakeups
```

## Per-5QI Monitoring Profiles

Per-5QI policy maps each configured service class to a monitoring profile:
- **FULL**: Never sparse.
- **ADAPTIVE**: Timer-based switching.
- **AGGRESSIVE**: Switching plus skipping.

The strictest profile among a UE's active bearers wins.

# Cross-References

- [FEATURE OVERVIEW](feature-overview.md)
- [PARAMETERS](parameters.md)
