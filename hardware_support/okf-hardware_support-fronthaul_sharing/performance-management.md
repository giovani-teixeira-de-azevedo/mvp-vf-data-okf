---
type: concept
resource: data/vodafone-mvp/raw/Fronthaul Sharing.pdf#performance-management
title: Performance Management
description: Details the performance management, KPIs, formulas, and counters for
  Fronthaul Sharing path monitoring.
tags:
- fronthaul sharing
- performance management
- KPIs
- counters
- telecom
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:43:44+00:00'
  source_sha256: 5aeaa779bccdf09d
sources:
- title: Fronthaul Sharing
  resource: data/vodafone-mvp/raw/Fronthaul Sharing.pdf
---

This section details the Performance Management (PM) framework for Fronthaul Sharing, specifying the key performance indicators (KPIs), performance counters, and monitoring guidelines. It outlines the metrics needed to assess path health, capacity utilization, and shared-fate risks over 15-minute Reporting Observation Periods (ROPs).

## Monitoring Framework

Fronthaul sharing performance monitoring is structured to answer three primary operational questions:
1. **Shared Path Health**: Is the path healthy in terms of delay and continuity?
2. **Capacity Partitioning**: Is the allocated capacity partition adequate (checking utilization and policing drops)?
3. **Shared-Fate Risk**: Is the shared-fate risk materializing (tracking guest outage minutes caused by host events)?

Counters must be collected per shared segment over 15-minute ROPs. The path delay must be baselined at integration. Because packet fronthaul delay is deterministic, a drift of $5\ \mu\text{s}$ or more is considered a meaningful event, typically indicating a re-routed fiber path or a failing optic.

## Key Performance Indicators (KPIs)

* **Guest Path Availability**: This metric should closely track the host node's availability. Any gap between guest and host path availability indicates path problems independent of host restarts, such as issues with optics or connectors.
* **Guest Utilization**: When guest utilization exceeds 80% of `guestMaxBandwidth` (defined in [Parameters](parameters.md)) during the busy hour, it triggers a requirement to re-dimension the bandwidth partition before policing drops occur.
* **Policing Drop Ratio**: Any non-zero value is an action item, as dropped fronthaul packets translate directly into air-interface errors for the guest's cells.
* **Path Delay Drift**: Tracks in-ROP delay variation.

### KPI Formulas

| KPI | Formula | Description |
| :--- | :--- | :--- |
| **Guest Path Availability** | $(1 - \text{ctrSharedPathDownTime} / 900) \times 100$ | Per-ROP availability of the shared path (%) |
| **Guest Utilization** | $\frac{\text{ctrGuestOctets} \times 8}{900 \times \text{guestMaxBandwidth} \times 10^9} \times 100$ | Guest share-of-reservation usage (%) |
| **Policing Drop Ratio** | $\text{ctrGuestPolicedDrops} / \text{ctrGuestPackets} \times 100$ | Guest fronthaul packets dropped by policing (%) |
| **Path Delay Drift** | $\text{ctrPathDelayMax} - \text{ctrPathDelayMin}$ | In-ROP delay variation (ns), should be $< 1000$ |

## Performance Counters

The delay counters are sampled from continuous path-delay measurements. The counter `ctrPathDelayMax` requires close attention on TDD-sharing segments, as a delay excursion beyond the budgeted limit will corrupt the guest's Time Division Duplex (TDD) switching alignment before an alarm is raised. 

The counter `ctrHostInducedOutage` is used to isolate shared-fate downtime, allowing host maintenance events to be distinguished from genuine path faults in availability reporting.

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
| `ctrGuestOctets` | Guest fronthaul octets forwarded per ROP | $0\text{ to } 2^{63}$ | `int64` |
| `ctrGuestPackets` | Guest fronthaul packets forwarded per ROP | $0\text{ to } 2^{63}$ | `int64` |
| `ctrGuestPolicedDrops` | Guest packets dropped by bandwidth policing | $0\text{ to } 2^{31}$ | `int64` |
| `ctrSharedPathDownTime` | Accumulated shared-path unavailable time per ROP | $0\text{ to } 900\ \text{s}$ | `int64` |
| `ctrHostInducedOutage` | Portion of downtime caused by host restarts | $0\text{ to } 900\ \text{s}$ | `int64` |
| `ctrPathDelayMin` | Minimum measured one-way path delay per ROP | $0\text{ to } 2^{31}\ \text{ns}$ | `int64` |
| `ctrPathDelayMax` | Maximum measured one-way path delay per ROP | $0\text{ to } 2^{31}\ \text{ns}$ | `int64` |
| `ctrKeepAliveLoss` | Keep-alive messages lost | $0\text{ to } 2^{31}$ | `int64` |

# Cross-References

* [Parameters](parameters.md) for details on bandwidth configuration parameters like `guestMaxBandwidth`.
