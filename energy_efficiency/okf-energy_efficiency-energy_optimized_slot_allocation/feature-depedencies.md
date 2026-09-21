---
type: concept
resource: data/vodafone-mvp/raw/Energy-Optimized Slot Allocation.pdf#feature-depedencies
title: Feature Dependencies
description: Prerequisites, hardware and network dependencies, feature interactions,
  and limitations of the Energy-Optimized Slot Allocation feature.
tags:
- energy-saving
- slot-allocation
- dependencies
- limitations
- license
- micro-sleep
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:40:17+00:00'
  source_sha256: 1d762a2e6b1b7bcc
sources:
- resource: data/vodafone-mvp/raw/Energy-Optimized Slot Allocation.pdf
  title: Energy-Optimized Slot Allocation
---

The Energy-Optimized Slot Allocation feature changes the scheduler packing strategy to feed empty slots to the micro-sleep machinery. This section details the prerequisite licenses, software interactions, hardware/network requirements, and operational limitations that must be reviewed per cell prior to deployment.

## Feature Dependencies and Interactions

### License and Control Parameter
* **License Requirement:** Requires a valid license key **FAK-31535** installed.
* **Control Parameter:** The feature is enabled by setting `FeatureCtrl=EnergyOptSlotAlloc` to `ACTIVATED`.

### Feature Interworking and Interactions
* **NR Micro Sleep Tx:** Strongly recommended to be enabled together with this feature. Without NR Micro Sleep Tx, empty slots only yield marginal energy savings from digital front-end clock gating.
* **Energy-Optimized Symbol Allocation:** Interworks with this feature. Symbol compaction is applied within the slots that remain occupied.
* **Latency-Sensitive Scheduling Features:** Bearers under Latency-Prioritized Scheduling or NR Delay-Controlled Scheduling are exempt from slot batching.
* **Minimum Inter-Cell Interference Scheduling:** Dense-slot packing concentrates interference in time. When both features are active, the interference-aware PRB placement operates within the batched slots.

## Hardware and Network Dependencies

### Hardware Dependencies
* **Scheduling Function:** No dedicated hardware is required for the scheduling function itself.
* **Radio Capabilities:** Realized energy savings depend on the radio's micro-sleep capability. All current radio generations support micro-sleep, but deeper multi-slot sleep requires generation R2 or later.

### Network Dependencies
* **Coordination:** None. The feature is cell-local. In synchronized TDD networks, the batching pattern is independent per cell and requires no inter-node coordination.

## Limitations

* **Downlink Only:** Batching applies to the downlink scheduler only. Uplink scheduling remains unchanged (see NR Micro Sleep Rx for the receive side).
* **Queuing Delay Cap:** The maximum added queuing delay introduced by the batching algorithm is capped at **8 ms** regardless of configuration.
* **High-Load No-Op:** At downlink PRB utilization above approximately **40%**, batching opportunities disappear naturally, and the feature becomes a no-op (it neither benefits nor harms performance at high load).
* **Pinned Slots:** SSB, SIB1, paging occasions, and RACH response windows pin their respective slots; these slots are never moved by the scheduler packing algorithm.

# Cross-References

* [Feature Overview](feature-overview.md) - For an overview of the scheduler packing strategy and energy savings concepts.
* [Feature Operation](feature-operation.md) - For technical details on slot batching and the compaction mechanism.
* [Parameters](parameters.md) - For the description of `FeatureCtrl` and other configuration options.
* [Activation Procedure](activation-procedure.md) - For step-by-step instructions on enabling `FeatureCtrl=EnergyOptSlotAlloc`.
