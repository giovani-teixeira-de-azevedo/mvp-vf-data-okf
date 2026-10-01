---
type: concept
resource: data/vodafone-mvp/raw/Coverage-Optimized Uplink Transmission High-Band.pdf#feature-operation
title: Feature Operation
description: Describes the operational mechanisms, thresholds, and state transitions
  of the Coverage-Optimized Uplink Transmission High-Band feature.
tags:
- uplink-coverage
- high-band
- gNodeB
- RRC-reconfiguration
- PUSCH-repetition
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T11:14:32+00:00'
  source_sha256: ac29c130b894e39e
sources:
- title: Coverage-Optimized Uplink Transmission High-Band
  resource: data/vodafone-mvp/raw/Coverage-Optimized Uplink Transmission High-Band.pdf
---

This section describes the operational mechanisms, thresholds, and state transitions of the Coverage-Optimized Uplink Transmission High-Band feature. It details how the gNodeB monitors uplink link quality and triggers coverage-optimized mode, including RRC reconfiguration, PUSCH repetition, and mobility prioritization.

## Operational Mechanism

### Uplink Quality Estimation
The gNodeB estimates the uplink link quality per active beam using Sounding Reference Signal (SRS) and Physical Uplink Shared Channel (PUSCH) Demodulation Reference Signal (DMRS) measurements, filtered over a 100 ms window.

### Coverage-Optimized Mode Entry
When the filtered Signal-to-Interference-plus-Noise Ratio (SINR) falls below the threshold `covEnterThr`, the UE enters coverage-optimized mode. The scheduler performs the following actions:
*   Switches the UE to Discrete Fourier Transform Spread Orthogonal Frequency Division Multiplexing (DFTS-OFDM) via Radio Resource Control (RRC) reconfiguration of `transformPrecoder`.
*   Restricts uplink allocations to at most `maxPrbCovMode` Physical Resource Blocks (PRBs).
*   Applies a Modulation and Coding Scheme (MCS) ceiling.

### PUSCH and PUCCH Repetition
If the SINR continues to fall below `repEnterThr`, slot-aggregated PUSCH repetition is activated. The repetition factor scales with the SINR deficit, up to `maxPuschRep`. Simultaneously, Physical Uplink Control Channel (PUCCH) resources are remapped to long formats with repetition to ensure that Hybrid Automatic Repeat Request (HARQ) feedback for measurement reports survives.

## Sequence Diagram

The following sequence diagram illustrates the transition into and out of coverage-optimized mode:

```mermaid
sequenceDiagram 
    participant UE 
    participant SCH as gNodeB Scheduler 
    participant RRC as gNodeB RRC 
    SCH->>SCH: Filtered UL SINR < covEnterThr 
    SCH->>RRC: Request coverage-mode reconfiguration 
    RRC->>UE: RRCReconfiguration (transformPrecoder=enabled, pusch-AggregationFactor) 
    UE-->>RRC: RRCReconfigurationComplete 
    SCH->>UE: UL grants (narrowband, MCS-capped, K repetitions) 
    UE->>SCH: PUSCH with K slot repetitions 
    Note over UE,SCH: MeasurementReports delivered reliably at cell edge 
    SCH->>SCH: Filtered UL SINR > covExitThr (hysteresis) 
    RRC->>UE: RRCReconfiguration (restore normal format)
```

## Exit Conditions and Hysteresis

To prevent reconfiguration ping-pong, the entry and exit thresholds are asymmetric:
*   **Exit Criteria**: Exiting the coverage-optimized mode requires the filtered uplink SINR to stay above `covExitThr` for the duration specified by `covExitTimer`.
*   **Restoration**: Once the exit criteria are met, the gNodeB RRC sends an `RRCReconfiguration` message to restore the normal format.

## Mobility Prioritization

Mobility procedures are prioritized over the gradual ramp-up of resources:
*   Once a measurement report configured for A3 or A5 events is pending, the UE is immediately granted repetition resources regardless of the gradual ramp.

# Cross-References

*   [Feature Overview](feature-overview.md)
*   [Parameters](parameters.md)
