---
type: concept
resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf#parameters
title: PARAMETERS
description: Configuration parameters for Closed-Loop Power Control High-Band set
  per NR sector carrier.
tags:
- closed-loop power control
- high-band
- nr
- parameters
- sinr
- fr2
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T12:54:16+00:00'
  source_sha256: c7e81fe336ed2a07
sources:
- resource: data/vodafone-mvp/raw/Closed-Loop Power Control High-Band.pdf
  title: Closed-Loop Power Control High-Band
---

This section specifies the configuration parameters for Closed-Loop Power Control High-Band within the NR sector carrier scope.

The parameters below are set per NR sector carrier. The target SINR is the primary tuning knob: raising it improves per-UE MCS at the cost of higher interference; the default is chosen for dense urban FR2 grids. The hysteresis window and step configuration should only be changed after observing loop stability counters.

| Parameter | Description | Values | Datatype | Default |
| --- | --- | --- | --- | --- |
| `closedLoopPcEnabled` | Enables the closed loop on the sector carrier | true, false | boolean | false |
| `pcTargetSinr` | Target uplink SINR for the control loop | -5–30 (dB) | int32 | 12 |
| `pcHysteresis` | Half-width of the no-action window around the target | 0–6 (dB) | int32 | 1 |
| `pcStepUp` | TPC step for below-target correction | 1, 3 (dB) | enum | 1 |
| `pcStepDown` | TPC step for above-target correction | 1 (dB) | enum | 1 |
| `blockageDetectThr` | SINR drop triggering fast ramp-up mode | 6–20 (dB) | int32 | 10 |
| `loopResetOnBeamSwitch` | Reset accumulated state at beam change | true, false | boolean | true |
| `srsWeight` | Weight of SRS vs PUSCH DMRS in the SINR filter | 0–100 (%) | int32 | 30 |
