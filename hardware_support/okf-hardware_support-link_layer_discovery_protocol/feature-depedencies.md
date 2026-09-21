---
type: concept
resource: data/vodafone-mvp/raw/Link Layer Discovery Protocol.pdf#feature-depedencies
title: Feature Dependencies
description: Details the software, hardware, and network dependencies as well as known
  limitations of the Link Layer Discovery Protocol (LLDP) feature.
tags:
- lldp
- dependencies
- limitations
- network-requirements
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:44:24+00:00'
  source_sha256: 3f29119d8593ada4
sources:
- title: Link Layer Discovery Protocol
  resource: data/vodafone-mvp/raw/Link Layer Discovery Protocol.pdf
---

This section details the dependencies and limitations associated with the Link Layer Discovery Protocol (LLDP) feature. It covers software, hardware, and network-level requirements, alongside specific operational constraints.

## Planning Considerations

Dependencies are minimal, reflecting the advisory nature of LLDP. The primary planning consideration is on the far-side device:
* The neighbor table is only as complete as the LLDP support of the connected routers and switches.
* Some operators deliberately disable LLDP on security-hardened router ports.

## Feature Dependencies

* **License Requirements:** Part of the base software package; no license key is required. Control and activation are managed via `FeatureCtrl=Lldp` under `TransportFunction=1` using `featureState` activation.
* **Zero Touch Integration (ZTI):** Consumes LLDP neighbor data for cabling verification when available.
* **Ethernet Link Aggregation (LAG):** The IEEE 802.3 link aggregation TLV reflects LAG membership, aiding the verification of aggregated links.
* **Feature Coexistence:** No conflicts with other features. LLDP frames coexist with all traffic classes.

## Hardware Dependencies

* **Supported Ports:** Available on all Ethernet transport ports of all baseband generations and on packet fronthaul ports of fronthaul-switch-capable units.
* **Unsupported Ports:** No support on CPRI ports, as they are not Ethernet-framed.

## Network Dependencies

* **Far-End Capability:** Far-end devices must have LLDP enabled for bidirectional visibility; however, the node's own advertisements are independent of far-end support.
* **Intermediate Nodes:** Intermediate unmanaged media converters are transparent to LLDP and are therefore invisible in the network topology (the neighbor seen by the node is the active device located beyond the converter).

## Limitations

* **Neighbor Limit:** One neighbor is stored per port by default. Ports connected to shared segments (such as hubs or unmanaged switches) may see multiple neighbors, of which only the most recent four are retained.
* **LLDP-MED:** LLDP-Media Endpoint Discovery (LLDP-MED) extensions (such as voice VLAN or PoE negotiation) are not implemented. Only baseline IEEE 802.1AB and selected IEEE 802.3 TLVs are supported.
* **Data Trustworthiness:** Received TLVs are stored verbatim as untrusted data. The node never acts automatically on neighbor-advertised values.

# Cross-References

* [Feature Overview](feature-overview.md) — For general details about the LLDP feature.
* [Feature Operation](feature-operation.md) — For details on LLDP frame transmission, reception, and TLVs.
* [Parameters](parameters.md) — For configuration details including `FeatureCtrl=Lldp`.
* [Activation Procedure](activation-procedure.md) — For details on activating the LLDP feature.
