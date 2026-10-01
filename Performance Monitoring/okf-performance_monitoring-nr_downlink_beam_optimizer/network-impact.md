---
type: concept
resource: data/vodafone-mvp/raw/NR Downlink Beam Optimizer.pdf#network-impact
title: Network Impact
description: Outlines the network impacts across coverage, mobility, end-user experience,
  and key performance indicators (KPIs).
tags:
- Network Impact
- Coverage
- Mobility
- KPIs
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T17:05:29+00:00'
  source_sha256: 9cde97df0022b505
sources:
- resource: data/vodafone-mvp/raw/NR Downlink Beam Optimizer.pdf
  title: NR Downlink Beam Optimizer
---

This section describes the network impacts on coverage, mobility, end users, and KPIs.

### Coverage
* **Traffic-Weighted RSRP:** Typically improves by 1–3 dB.
* **Sector-Edge Coverage:** Nominal sector-edge coverage is preserved by constraint.
* **Footprint Shape:** The footprint shape changes. It is recommended to verify border areas after major changes.

### Mobility
* **Beam-Switch Rates:** Typically drop as beams align with traffic.
* **Handover Borders:** Neighbor-cell handover borders can shift slightly. A mobility KPI watch is warranted for 48 hours after each change.

### End Users
* **SSB Gap:** A sub-second SSB gap occurs at each grid change.
* **Overall Impact:** Otherwise, the impact is positive, resulting in better RSRP and fewer beam failures.

### KPIs
* **Per-Beam Counters:** Time series break at each grid change because beam identities change.
* **Analytics:** Analytics must key on the grid version, which is exported in `ctrGridVersion`-tagged records.
