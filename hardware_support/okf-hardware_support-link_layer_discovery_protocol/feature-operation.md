---
type: concept
resource: data/vodafone-mvp/raw/Link Layer Discovery Protocol.pdf#feature-operation
title: Feature Operation
description: Describes the transmit, receive, validation, and port state behaviors
  of the LLDP agent.
tags:
- lldp
- feature-operation
- ran-transport
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:44:27+00:00'
  source_sha256: d08e035e3fc79768
sources:
- resource: data/vodafone-mvp/raw/Link Layer Discovery Protocol.pdf
  title: Link Layer Discovery Protocol
---

This section describes the operational behavior of the Link Layer Discovery Protocol (LLDP) agent, including packet transmission, receiving and validation mechanisms, port management modes, and performance overhead.

## Transmit Behavior

Once LLDP is enabled on a port, the agent handles transmission of LLDP advertisements as follows:
- **Periodic Transmission:** Advertisements are sent every `txInterval` seconds.
- **Fast-Transmit Burst:** Immediately after a link-up or a local configuration change, a fast-transmit burst of one frame per second is initiated to ensure rapid neighbor convergence after interventions.
- **Time-to-Live (TTL):** Each advertisement carries a TTL calculated as:
  $$\text{TTL} = \text{txInterval} \times \text{txHoldMultiplier}$$
  If a neighbor entry is not refreshed within this TTL, it is aged out and a topology-change event is logged.
- **Link-Down Event:** On link-down, a shutdown advertisement with a TTL of 0 is sent where possible, causing the peer to remove the entry immediately rather than waiting for age-out.

## Receive Behavior and Validation

The LLDP agent processes incoming frames on active ports as follows:
- **Validation:** Received frames are validated to ensure they have the correct destination MAC address and well-formed mandatory Type-Length-Value (TLV) fields.
- **Storage:** Validated frames are stored in the per-port neighbor table along with a receive timestamp.
- **Access and Monitoring:** 
  - The neighbor table is readable via `rancli` and northbound via the normal configuration/state interface.
  - Table changes generate state-change notifications that topology-management applications subscribe to.

## Port Administration Modes

A per-port `adminStatus` parameter selects the operational mode of the port. This accommodates security policies that allow learning the far end without advertising node identity outward:
- **Transmit-only**
- **Receive-only**
- **Bidirectional**

## Resource Overhead

The processing cost of LLDP is negligible. At default timers, LLDP generates one 200–400 byte frame per port per 30 seconds.

# Cross-References

* [Feature Overview](feature-overview.md) — For a high-level description of the LLDP feature.
* [Parameters](parameters.md) — For detailed configuration parameters such as `txInterval` and `txHoldMultiplier`.
