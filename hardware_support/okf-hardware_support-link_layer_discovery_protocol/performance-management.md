---
type: concept
resource: data/vodafone-mvp/raw/Link Layer Discovery Protocol.pdf#performance-management
title: Performance Management
description: Performance management metrics, KPIs, and counters for the Link Layer
  Discovery Protocol (LLDP) to monitor protocol health and topology stability.
tags:
- LLDP
- Performance Management
- KPIs
- Counters
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:44:31+00:00'
  source_sha256: 3f75188eb0e9cac0
sources:
- title: Link Layer Discovery Protocol
  resource: data/vodafone-mvp/raw/Link Layer Discovery Protocol.pdf
---

Performance Management (PM) for the Link Layer Discovery Protocol (LLDP) is designed to monitor protocol health and network topology stability rather than raw traffic volume. Key monitoring objectives include verifying cabling completeness, ensuring neighbor stability, and detecting malformed frames.

Counters are accumulated per port over a 15-minute Reporting Period (ROP). The baseline expectation after successful integration is that every provisioned port shows exactly one stable neighbor and zero discarded frames, indefinitely.

## Key Performance Indicators (KPIs)

*   **Neighbor Stability:** This is the primary KPI for monitoring topology health. A healthy port should experience zero ageouts. Recurring ageouts indicate a marginal link that is dropping frames or a far-end LLDP agent that is intermittently restarting, both of which require investigation before affecting service.
*   **Discard Ratio:** This metric should remain at exactly zero. Non-zero values typically indicate a non-compliant device model on the far end, providing valuable information for transport equipment audits.
*   **Advertisement Continuity:** This measures whether LLDP frames are being sent at the expected frequency based on the transmission interval configuration.

### KPI Formulas

| KPI | Formula | Description |
| :--- | :--- | :--- |
| **Neighbor Stability** | `ctrLldpAgeouts + ctrLldpNeighborChanges` | Topology change events per ROP (Target: 0) |
| **Discard Ratio** | `(ctrLldpFramesDiscarded / ctrLldpFramesInTotal) * 100` | Share of received LLDP frames discarded (%) |
| **Advertisement Continuity** | `(ctrLldpFramesOutTotal / (900 / txInterval)) * 100` | Sent frames vs expected per ROP (%) |

> *Note: The parameter `txInterval` used in the Advertisement Continuity formula represents the LLDP transmit interval (see [Parameters](parameters.md)).*

## LLDP Counters

The LLDP performance counters mirror the IEEE 802.1AB statistics group. 

Unrecognized TLVs (`ctrLldpTlvsUnrecognized`) are informational because unknown TLVs are legally skipped. However, a sudden spike in unrecognized TLVs can help correlate and timestamp software upgrades on far-end network elements.

| Counter | Description | Range | Datatype |
| :--- | :--- | :--- | :--- |
| `ctrLldpFramesOutTotal` | LLDP frames transmitted | 0–2³¹ | int64 |
| `ctrLldpFramesInTotal` | LLDP frames received | 0–2³¹ | int64 |
| `ctrLldpFramesDiscarded` | Received frames discarded (validation failure) | 0–2³¹ | int64 |
| `ctrLldpTlvsUnrecognized` | Unrecognized TLVs skipped in valid frames | 0–2³¹ | int64 |
| `ctrLldpAgeouts` | Neighbor entries aged out (TTL expiry) | 0–2³¹ | int64 |
| `ctrLldpNeighborChanges` | Neighbor entries added or replaced | 0–2³¹ | int64 |

# Cross-References

*   [Feature Overview](feature-overview.md)
*   [Parameters](parameters.md) — For `txInterval` and other LLDP timer configurations.
