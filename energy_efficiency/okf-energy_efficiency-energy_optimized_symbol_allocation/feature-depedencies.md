---
type: concept
resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf#feature-depedencies
title: Feature Dependencies
description: Details the feature, hardware, network dependencies, and functional limitations
  for Energy-Optimized Symbol Allocation.
tags:
- energy-saving
- symbol-allocation
- dependencies
- limitations
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:41:08+00:00'
  source_sha256: c1714cfcce048cae
sources:
- resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf
  title: Energy-Optimized Symbol Allocation
---

This section outlines the feature, hardware, network dependencies, and functional limitations for the **Energy-Optimized Symbol Allocation** feature. It details prerequisites for enabling the feature and interactions with other cell-level configurations.

## Feature Dependencies

The feature alters the scheduler's time-domain resource allocation strategy and depends on the radio's symbol-level muting capability. Review interactions with other time-domain features per cell:

* **License and Configuration:** Requires a valid license key (`FAK-31540`) installed and the parameter `FeatureCtrl=EnergyOptSymbolAlloc` set to `ACTIVATED` (see [Parameters](parameters.md)).
* **NR Micro Sleep Tx:** This feature must be activated on the cell to realize energy savings; without it, the symbol compaction is energy-neutral.
* **Energy-Optimized Slot Allocation:** This feature complements Energy-Optimized Slot Allocation. Slot batching first concentrates data into fewer slots, and then symbol compaction shortens those slots.
* **NR Multiple PDCCH Symbols Low/Mid-Band:** Compaction respects the configured CORESET duration and never remaps control symbols.
* **Downlink Data and DMRS Multiplexing:** When both are active, additional Demodulation Reference Signal (DMRS) multiplexing gains apply within the compacted allocation.

## Hardware Dependencies

* **Power Amplifier (PA) Muting:** Symbol-level PA muting requires radio hardware generation **R2 or later**. On earlier radio hardware generations, the feature can be enabled but yields only digital front-end savings of approximately 1%.
* **Baseband Units:** Supported on all baseband units.

## Network Dependencies

* **Neighbor Coordination:** None. The feature is cell-local and does not require coordination with neighboring network nodes.

## Limitations

* **Blocking Probability:** Compaction widens the frequency-domain allocation, which at medium load increases the blocking probability for simultaneously scheduled UEs. To prevent scheduling degradation, the scheduler suspends compaction when more than `maxCompactionUsers` UEs contend per slot (see [Parameters](parameters.md)).
* **Excluded Transmission Types:** Compaction is not applied to broadcast transmissions (such as SIB and paging), Random Access Response (RAR), or Msg4 transmissions, which keep the cell-default time-domain allocation.
* **Reduced Bandwidth UEs:** UEs with reduced bandwidth capability (e.g., those supported by *NR Reduced UE Bandwidth Support*) cannot always absorb the frequency widening. For those UEs, compaction is limited to their active Bandwidth Part (BWP) width.
* **Minimum Compacted Length:** Bounded by DMRS overhead efficiency, the minimum compacted length is:
  * **4 symbols** under default conditions.
  * **2 symbols** for Mapping Type B with short data.

# Cross-References

* [Feature Operation](feature-operation.md) — For details on the compaction mechanism.
* [Parameters](parameters.md) — For `FeatureCtrl=EnergyOptSymbolAlloc` and `maxCompactionUsers` configuration details.
