---
type: concept
resource: data/vodafone-mvp/raw/Mixed Bandwidth Support for Carrier Aggregation High-Band.pdf#feature-overview
title: Feature Overview
description: Overview of the Mixed Bandwidth Support for Carrier Aggregation High-Band
  feature, which enables mixing different channel bandwidths in FR2 carrier aggregation
  configurations.
tags:
- carrier-aggregation
- high-band
- fr2
- mixed-bandwidth
- gnodeb
- mmwave
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:38:31+00:00'
  source_sha256: e0fb6aa4223e921a
sources:
- resource: data/vodafone-mvp/raw/Mixed Bandwidth Support for Carrier Aggregation
    High-Band.pdf
  title: Mixed Bandwidth Support for Carrier Aggregation High-
---

The **Mixed Bandwidth Support for Carrier Aggregation High-Band** feature removes the restriction requiring all high-band (FR2) component carriers (CCs) in an NR carrier aggregation (CA) configuration to use the same channel bandwidth. This allows operators to leverage irregular FR2 spectrum holdings and avoid leaving valuable spectrum stranded.

---

## Technical Description

In legacy configurations without this feature, the NR carrier aggregation machinery requires uniform carrier bandwidth across the FR2 CC set (typically 100 MHz per CC). This restriction forces operators with non-uniform or irregular FR2 spectrum holdings to either leave some blocks unused or forgo aggregating certain odd-sized blocks entirely. 

Irregular spectrum holdings are common in mmWave bands. Examples include:
* An operator holding a 400 MHz block in the n257 band, acquired as three 100 MHz blocks and two 50 MHz blocks ($3 \times 100 \text{ MHz} + 2 \times 50 \text{ MHz}$).
* A licensed block whose edge alignment only permits a split of 200 MHz plus 50 MHz.

With this feature enabled, the gNodeB can build CA combinations that mix **50 MHz, 100 MHz, 200 MHz, and 400 MHz** component carriers. The creation of these combinations is subject to:
* **UE capability signaling** for band-combinations per **3GPP TS 38.306**.
* **Fallback rules** defined in **3GPP TS 38.101-2**.

### Scheduling and Enforcement
* **Scheduling and Link Adaptation**: The gNodeB schedules and link-adapts each CC independently according to its individual channel bandwidth.
* **Aggregate Limits**: All aggregate performance limits—such as maximum aggregated bandwidth per UE, total CC count, and MIMO layer configurations—are enforced against actual per-UE capabilities rather than being restricted by a uniform carrier bandwidth assumption.

---

## Value and Benefits

* **Direct Spectral Efficiency**: Previously stranded 50 MHz blocks become usable as Secondary Cell (SCell) capacity.
* **Capacity and Throughput Gains**: On a typical irregular 350 MHz holding (e.g., $3 \times 100 \text{ MHz} + 1 \times 50 \text{ MHz}$), enabling mixed bandwidth CA recovers 50 to 100 MHz of aggregable spectrum. This translates to:
  * A **15% to 40% peak throughput increase** for capable UEs.
  * A proportional gain in busy-hour network capacity.
* **Simplified Refarming**: Carriers can be resized independently over time without breaking the existing CA combination.

---

## Mixed Bandwidth CA Architecture

Below is a logical representation of an irregular 350 MHz spectrum holding in the n257 band being aggregated under this feature:

```mermaid
flowchart LR 
    subgraph Spectrum holding n257 
        C1[CC1 100 MHz] 
        C2[CC2 100 MHz] 
        C3[CC3 100 MHz] 
        C4[CC4 50 MHz] 
    end 
    C1 --> AGG[Mixed BW CA engine] 
    C2 --> AGG 
    C3 --> AGG 
    C4 --> AGG 
    AGG --> UE[UE: 350 MHz aggregated<br/>previously 300 MHz]
```

---

# Cross-References

For details on configuration, dependencies, and deployment of this feature, refer to the following sections:
* [Feature Dependencies](feature-depedencies.md) — Prerequisites and co-requisite features.
* [Feature Operation](feature-operation.md) — Operational behavior and functional mechanics.
* [Network Impact](network-impact.md) — Observed performance impact on network resources and KPIs.
* [Parameters](parameters.md) — Configuration parameters to control and tune mixed bandwidth behavior.
* [Activation Procedure](activation-procedure.md) — Steps required to enable this feature on the gNodeB.
