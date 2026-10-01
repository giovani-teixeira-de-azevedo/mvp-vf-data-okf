---
type: concept
resource: data/vodafone-mvp/raw/NR Mobility.pdf#feature-operation
title: FEATURE OPERATION
description: Describes the mobility behavior, measurement configurations, execution
  steps, and filtering parameters for NR Mobility.
tags:
- NR Mobility
- Handover
- Measurement Configuration
- A3 Event
- A2 Event
- A1 Event
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T09:19:26+00:00'
  source_sha256: 6e917599d6bdc500
sources:
- title: NR Mobility
  resource: data/vodafone-mvp/raw/NR Mobility.pdf
---

This section describes the operational behavior of the NR Mobility feature, including measurement configurations, handover execution, and filtering parameters.

## Configuration Levels

Mobility behavior is configured at multiple levels of granularity:
* **Per Cell**: General mobility settings applied to the serving cell.
* **Per Frequency Relation**: Settings applied to relations between different frequencies.
* **Per-Cell-Relation Individual Offsets**: The finest tuning level, allowing individual offsets for specific cell relations.

---

## Measurement and Triggering

### Steady State
In steady state, an **intra-frequency A3 configuration** is always active to monitor and trigger intra-frequency handovers.

### Inter-Frequency Measurements
Inter-frequency measurements are dynamically configured and deactivated based on serving cell quality:
* **Activation (A2 Event)**: When the serving RSRP crosses the `a2SearchThr` threshold, inter-frequency measurement objects are configured for candidate relations (based on their priority and thresholds). Measurement gaps are configured if required by the UE.
* **Deactivation (A1 Event)**: When the serving RSRP reaches `a1StopThr`, the A1 event is triggered, and the measurement gaps are torn down.

---

## Handover Evaluation and Execution

1. **Report Evaluation**: Measurement reports are evaluated against the relation table. Blacklisted relations (where `noHo=true`) are ignored.
2. **Target Preparation**: The mobility engine prepares the chosen target cell.
3. **Execution**: The network sends an `RRCReconfiguration` message containing `reconfigurationWithSync` to the UE.
4. **Supervision**: The network supervises the handover execution using the T304 timer (`t304Timer`).
5. **Failure Handling**: Any handover failure is classified using the UE's re-establishment or Radio Link Failure (RLF) report to generate Mobility Robustness Optimization (MRO) statistics.

---

## Filtering and Hysteresis

Filtering and hysteresis parameters follow TS 38.331 specifications:
* **L3 Filtering Coefficient**: `filterCoeffRsrp`
* **Per-Event Hysteresis**: `hysteresisA3`
* **Time to Trigger**: `timeToTriggerA3`

### Profile Recommendations
* **Default Profile**: Implements a conservative macro profile.
* **Dense Urban Small-Cells**: Borders in dense urban small-cell environments usually require shorter Time to Trigger (TTT) and per-relation offsets to handle rapid signal degradation.

# Cross-References

* [Feature Overview](feature-overview.md)
* [Parameters](parameters.md)
* [Performance Management](performance-management.md)
