---
type: concept
resource: data/vodafone-mvp/raw/Extended Propagation Delay Support High-Band.pdf#network-impact
title: Network Impact
description: Details the impact of Extended Propagation Delay Support High-Band on
  coverage, capacity, interference, accessibility, and KPIs.
tags:
- network-impact
- coverage
- capacity
- interference
- accessibility
- kpi
- FR2
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:26:56+00:00'
  source_sha256: d4c25c2cd0522201
sources:
- title: Extended Propagation Delay Support High-Band
  resource: data/vodafone-mvp/raw/Extended Propagation Delay Support High-Band.pdf
---

This section outlines the network-level impacts of deploying the Extended Propagation Delay Support High-Band feature. It details key consequences for coverage, capacity, interference, accessibility, and KPIs.

## Network Impact Areas

* **Coverage:** 
  The FR2 usable range extends from approximately 2 km up to 10 km for link-budget-capable User Equipment (UEs), primarily Fixed Wireless Access Customer Premise Equipment (FWA CPEs). Consequently, FR2 Carrier Aggregation (CA) becomes available to distant subscribers.
* **Capacity:** 
  * **Uplink:** Capacity on the extended cell decreases by 1% to 2% due to longer physical random access channel (PRACH) and guard overhead.
  * **Downlink:** Capacity remains unchanged.
* **Interference:** 
  The wider receive window makes the cell more sensitive to distant same-channel interferers. It is recommended to verify the frequency reuse plan.
* **Accessibility:** 
  The Random Access (RA) success rate for distant UEs improves from near zero to normal levels. However, the overall RA success rate may appear to dip slightly because previously invisible distant connection attempts are now successfully registered and included in the statistics.
* **KPIs:** 
  Timing Advance (TA) distribution histograms shift right by design. Any range-based alarm thresholds should be updated accordingly.

# Cross-References

* [Feature Overview](feature-overview.md) — For more context on the capabilities of this feature.
* [Performance Management](performance-management.md) — For monitoring network performance and KPIs affected by this feature.
