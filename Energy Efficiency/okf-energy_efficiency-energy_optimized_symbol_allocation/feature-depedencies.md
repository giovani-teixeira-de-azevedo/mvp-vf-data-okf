---
type: concept
resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf#feature-depedencies
title: Feature Dependencies
description: Details the feature, hardware, and network dependencies as well as operational
  limitations for Energy-Optimized Symbol Allocation.
tags:
- energy-saving
- symbol-allocation
- dependencies
- limitations
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-29T22:41:41+00:00'
  source_sha256: c1714cfcce048cae
sources:
- resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf
  title: Energy-Optimized Symbol Allocation
---

This section outlines the software, hardware, and network dependencies for the Energy-Optimized Symbol Allocation feature, as well as its operational limitations and interactions with other features.

## Feature Dependencies

- **License and Control Parameter**: Requires a valid license key (`FAK-31540`) installed and `FeatureCtrl=EnergyOptSymbolAlloc` set to `ACTIVATED`.
- **NR Micro Sleep Tx**: Requires NR Micro Sleep Tx to be activated on the cell to realize the energy saving; without it, compaction is energy-neutral.
- **Energy-Optimized Slot Allocation**: Complements Energy-Optimized Slot Allocation; slot batching first concentrates data into fewer slots, and symbol compaction then shortens those slots.
- **NR Multiple PDCCH Symbols Low/Mid-Band**: Compaction respects the configured CORESET duration and never remaps control symbols.
- **Downlink Data and DMRS Multiplexing**: When both features are active, additional DMRS multiplexing gains apply within the compacted allocation.

## Hardware Dependencies

- **Power Amplifier (PA) Muting**: Symbol-level PA muting requires radio hardware generation R2 or later. On earlier radios, the feature can be enabled but yields only digital front-end savings (about 1%).
- **Baseband Units**: Supported on all baseband units.

## Network Dependencies

- **Neighbor Node Coordination**: None. The feature is cell-local and needs no coordination with neighbor nodes.

## Limitations

- **Frequency Widening & Contention**: Compaction widens the frequency allocation, which at medium load increases the blocking probability for simultaneously scheduled UEs. The scheduler suspends compaction when more than `maxCompactionUsers` UEs contend per slot.
- **Excluded Transmissions**: Not applied to broadcast (SIB, paging), RAR, or Msg4 transmissions, which keep the cell-default time-domain allocation.
- **UE Bandwidth Restrictions**: UEs with reduced bandwidth capability (see NR Reduced UE Bandwidth Support) cannot always absorb the frequency widening; for those UEs, compaction is limited to their active BWP width.
- **Symbol Allocation Bounds**: Minimum compacted length is 4 symbols (2 for mapping type B with short data), bounded by DMRS overhead efficiency.

# Cross-References

- [Feature Overview](feature-overview.md)
- [Parameters](parameters.md)
- [Activation Procedure](activation-procedure.md)
