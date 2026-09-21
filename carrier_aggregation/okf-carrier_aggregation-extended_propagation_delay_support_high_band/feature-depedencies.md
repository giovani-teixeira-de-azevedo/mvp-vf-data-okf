---
type: concept
resource: data/vodafone-mvp/raw/Extended Propagation Delay Support High-Band.pdf#feature-depedencies
title: Feature Dependencies
description: Dependencies and limitations of the Extended Propagation Delay Support
  High-Band feature, covering license, hardware, network, and operational constraints.
tags:
- dependencies
- hardware-requirements
- licensing
- limitations
- fr2
- high-band
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:26:58+00:00'
  source_sha256: dd50f3a52130f0fa
sources:
- resource: data/vodafone-mvp/raw/Extended Propagation Delay Support High-Band.pdf
  title: Extended Propagation Delay Support High-Band
---

This section outlines the feature dependencies, hardware requirements, network impacts, and functional limitations for the Extended Propagation Delay Support High-Band feature. Because this feature modifies Physical Random Access Channel (PRACH) configurations, baseband receive windows, and Medium Access Control (MAC) timing control on FR2 carriers, alignment with the physical layer baseline and related features is critical.

## Dependencies and Interactions

### Feature Dependencies
- **Physical Layer**: Requires *Physical Layer High-Band* to be activated on the node.
- **Licensing and Control**: Requires a valid license key (`FAK-33014`) and the parameter `FeatureCtrl=ExtPropDelayHighBand` set to `ACTIVATED`.
- **Low-Band Companion**: This is a companion feature to *Extended Propagation Delay Support Low-Band* (Radio Access category), which covers FR1. The two features are licensed separately.
- **Carrier Aggregation (CA)**: When the FR2 cell is used as a Secondary Cell (SCell), the CA baseline (*NR DL Carrier Aggregation* or *NR 4CC DL Carrier Aggregation High-Band*) must be active. Extended Timing Advance (TA) is then applied at SCell addition.
- **High Speed UE High-Band**: This feature interacts with *High Speed UE High-Band*. While the two features may be combined, the combined Doppler-plus-delay budget reduces the maximum supported UE speed at maximum range.

### Hardware Dependencies
- **Radio Units**: Supported on FR2 radio units of hardware generation R2 or later. Earlier units lack the necessary extended receive window buffering.
- **Baseband Units**: Require the extended-range uplink processing option (which is standard on high-capacity variants).

### Network Dependencies
- **Interference Footprint**: Cell range extension changes the interference footprint toward neighboring FR2 cells on the same channel. The `maxCellRange` parameter must be coordinated with the frequency plan.
- **Core and Transport**: There are no core or transport network dependencies.

## Feature Limitations
- **Maximum Configurable Range**: The maximum configurable range is limited to 10 km. Beyond this range, the FR2 link budget is rarely closable, even when using Customer Premises Equipment (CPE) antennas.
- **Uplink Capacity Reduction**: The longer PRACH occupies more uplink symbols, which reduces overall uplink capacity by approximately 1–2% depending on the configured TDD pattern.
- **SRS Incompatibility**: Extended range and *NR SRS Capacity Boost Mid-Band*-style Sounding Reference Signal (SRS) densification cannot be combined on the same carrier.
- **Beam-based Mobility**: Beam-based mobility measurement periodicity is not extended. Consequently, very distant, fast-moving UEs may experience delayed beam updates.

# Cross-References
- [Feature Overview](feature-overview.md)
- [Activation Procedure](activation-procedure.md)
- [Parameters](parameters.md)
- [Network Impact](network-impact.md)
