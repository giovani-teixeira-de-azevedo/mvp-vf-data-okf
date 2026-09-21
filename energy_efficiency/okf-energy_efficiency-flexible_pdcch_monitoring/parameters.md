---
type: reference-table
resource: data/vodafone-mvp/raw/Flexible PDCCH Monitoring.pdf#parameters
title: Parameters
description: Cell and profile configuration parameters for the Flexible PDCCH Monitoring
  feature.
tags:
- pdcch
- parameters
- configuration
- sssg
- energy-saving
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:41:44+00:00'
  source_sha256: b1fbfe543e0b13a7
sources:
- resource: data/vodafone-mvp/raw/Flexible PDCCH Monitoring.pdf
  title: Flexible PDCCH Monitoring
---

This section details the cell-level parameters and configuration interfaces used to control and tune the Flexible PDCCH Monitoring feature.

The inactivity timer and the sparse period periodicity act as the primary tuning pair for balancing energy conservation and signaling overhead:
* **Tuning Trade-offs**: A shorter `sssgSwitchTimer` maximizes UE power savings but may cause excessive group switching under chatty traffic profiles. The default configuration is optimized for standard smartphone traffic mixes.
* **QoS Policy Interface**: The `profile5qiMap` parameter defines the policy mapping per 5QI. It is recommended to keep latency-sensitive traffic such as voice and interactive 5QIs configured to `FULL` or `ADAPTIVE`.

## Parameter Configuration

| Parameter | Description | Values | Datatype | Default |
| :--- | :--- | :--- | :--- | :--- |
| `pdcchMonitoringMode` | Enables the function on the cell. | `DISABLED`, `SSSG_ONLY`, `SSSG_AND_SKIP` | enum | `DISABLED` |
| `sparsePeriod` | SSSG1 monitoring periodicity. | 2–16 (slots) | int32 | 4 |
| `sssgSwitchTimer` | Inactivity duration before switching to the sparse group. | 1–100 (ms) | int32 | 8 |
| `maxSkipDuration` | Maximum PDCCH skip window duration. | 2–40 (ms) | int32 | 20 |
| `profile5qiMap` | 5QI-to-profile mapping. | `FULL`, `ADAPTIVE`, `AGGRESSIVE` per 5QI | string | `"1:FULL,5:ADAPTIVE,9:AGGRESSIVE"` |
| `retxPinDense` | Pins the UE to the dense monitoring group while a HARQ retransmission is pending. | `false`, `true` | boolean | `true` |
| `minUeReportGap` | Minimum gap preserved for periodic CSI and SRS reporting occasions. | 0–20 (slots) | int32 | 4 |

# Cross-References

* [Feature Operation](feature-operation.md) — Explains how these parameters govern the switching behavior between search space sets and PDCCH skipping.
* [Activation Procedure](activation-procedure.md) — Details how to configure these parameters during cell deployment.
