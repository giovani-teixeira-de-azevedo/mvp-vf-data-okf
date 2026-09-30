---
type: concept
resource: data/vodafone-mvp/raw/Energy-Optimized Slot Allocation.pdf#feature-depedencies
title: Feature Dependencies
description: Details feature, hardware, and network dependencies and limitations for
  Energy-Optimized Slot Allocation.
tags:
- energy-saving
- slot-allocation
- dependencies
- limitations
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-30T09:38:32+00:00'
  source_sha256: 1d762a2e6b1b7bcc
sources:
- resource: data/vodafone-mvp/raw/Energy-Optimized Slot Allocation.pdf
  title: Energy-Optimized Slot Allocation
---

The Energy-Optimized Slot Allocation feature changes scheduler packing strategy to feed empty slots to the micro-sleep machinery. It has interactions with latency-sensitive scheduling features that must be reviewed per cell.

## Feature Dependencies

- Requires a valid license key (`FAK-31535`) installed and `FeatureCtrl=EnergyOptSlotAlloc` set to `ACTIVATED`.
- Strongly recommended together with **NR Micro Sleep Tx**; without it, empty slots yield only marginal savings from digital front-end clock gating.
- Interworks with **Energy-Optimized Symbol Allocation**: symbol compaction is applied within the slots that remain occupied.
- Bearers under **Latency-Prioritized Scheduling** or **NR Delay-Controlled Scheduling** are exempt from batching.
- Interacts with **Minimum Inter-Cell Interference Scheduling**: dense-slot packing concentrates interference in time; when both features are active, the interference-aware PRB placement operates within the batched slots.

## Hardware Dependencies

- No dedicated hardware for the scheduling function itself. The realized energy saving depends on the radio's micro-sleep capability (all current radio generations; deeper multi-slot sleep requires generation R2 or later).

## Network Dependencies

- None. Cell-local. In synchronized TDD networks, the batching pattern is independent per cell; no inter-node coordination is required.

## Limitations

- Batching applies to downlink only; uplink scheduling is unchanged (see **NR Micro Sleep Rx** for the receive side).
- Maximum added queuing delay is capped at 8 ms regardless of configuration.
- At downlink PRB utilization above approximately 40%, batching opportunities disappear naturally and the feature becomes a no-op; it neither helps nor harms at high load.
- SSB, SIB1, paging occasions, and RACH response windows pin their slots and are never moved.
