---
type: concept
resource: data/vodafone-mvp/raw/NR Automated Neighbor Relations.pdf#feature-depedencies
title: Feature Dependencies
description: Describes the feature, hardware, and network dependencies, as well as
  limitations of the NR Automated Neighbor Relations (ANR) feature.
tags:
- ANR
- Dependencies
- Limitations
- NR
- gNodeB
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T16:53:47+00:00'
  source_sha256: 8ea7b468cb7221a0
sources:
- resource: data/vodafone-mvp/raw/NR Automated Neighbor Relations.pdf
  title: NR Automated Neighbor Relations
---

This section describes the dependencies and limitations of the NR Automated Neighbor Relations (ANR) feature, including hardware, network, and software requirements. ANR touches RRC measurement configuration, the NG interface, and Xn establishment, so its dependency set spans the UE population, the core network, and inter-node transport. It is a foundational feature: most mobility features in this category assume ANR is active.

## Feature Dependencies

*   **NR Mobility:** Requires NR Mobility activated (ANR discovers relations; NR Mobility uses them).
*   **License and Activation:** Requires a valid license key (`FAK-33130`) installed and `FeatureCtrl=NrAnr` set to `ACTIVATED`.
*   **Inter-RAT Relation Discovery:** Supports EPS Fallback for IMS Voice and NR Emergency Fallback to LTE target quality.
*   **Xn/X2 Establishment:** Automatic Xn establishment interworks with Xn Configuration (NR); X2 relations for EN-DC are handled by X2 Configuration (EN-DC).
*   **PCI Conflict Information:** PCI conflict information discovered via CGI reads is forwarded to NR PCI Conflict Reporting.

## Hardware Dependencies

*   None. ANR is baseband software only and supported on all baseband units.

## Network Dependencies

*   **AMF Support:** The AMF must support NG Configuration Transfer (TS 38.413) for automatic Xn TNL address resolution.
*   **IP Connectivity:** IP connectivity (and firewall/IPsec policy) must permit Xn SCTP associations between gNodeBs; without it, relations are created but remain intra-AMF (NG) handover only.
*   **UE Support:** UEs must support `reportCGI` with autonomous gaps (the vast majority of commercial UEs do); ANR throughput scales with the share of capable UEs.

## Limitations

*   **CGI Reading Impact:** CGI reading requires the UE to leave the serving cell schedule during autonomous gaps; the gNodeB limits concurrent CGI orders per cell to `maxCgiOrdersPerCell` to bound the throughput impact.
*   **Non-served PLMNs:** Relations toward cells broadcasting only non-served PLMNs are recorded but marked `noHo` automatically.
*   **Automatic Removal:** Automatic removal never deletes relations marked `noRemove` or relations toward cells within the same gNodeB.
*   **Shared-RAN Deployments:** In shared-RAN deployments, relation creation follows the coordinating operator's policy; per-operator neighbor tables are not supported.

# Cross-References

*   [Feature Overview](feature-overview.md)
*   [Activation Procedure](activation-procedure.md)
