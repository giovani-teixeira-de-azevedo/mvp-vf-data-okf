---
type: concept
resource: data/vodafone-mvp/raw/Fronthaul Sharing.pdf#feature-operation
title: Feature Operation
description: Describes the integration, coordination handshake, and steady-state supervision
  processes of Fronthaul Sharing.
tags:
- fronthaul-sharing
- guest-node
- host-node
- coordination-handshake
- delay-measurement
- supervision
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:43:47+00:00'
  source_sha256: 1561daf6414a0681
sources:
- resource: data/vodafone-mvp/raw/Fronthaul Sharing.pdf
  title: Fronthaul Sharing
---

This section describes the operational mechanisms of the Fronthaul Sharing feature during integration, coordination handshake, and steady-state operation. It details how the host and guest basebands establish sharing sessions, negotiate carrier partitions, measure path delay, and maintain supervision.

## Integration and Coordination Handshake

During node integration, the hosting node discovers its fronthaul topology through standard procedures. The operator then configures the sharing arrangement:
- **Host Node Configuration:** Specific ports or radio units (RUs) are marked as shared, and the guest node identity is declared.
- **Guest Node Configuration:** Configured with a virtual fronthaul port referencing the host node.

Once the physical or logical link for the guest node is established, the host and guest nodes execute a coordination handshake consisting of the following steps:
1. **Capability Exchange:** Exchange of interface capabilities (e.g., CPRI/eCPRI types and supported rates).
2. **Delay Measurement:** Measurement of forwarding delay across the shared segment. The measured delay is reported back to both the host and guest basebands to be folded into their respective air-interface timing.
3. **Carrier Partition Agreement:** Negotiation and agreement of carrier partitions for dual-host radio units.

---

## Sequence Flow

The following sequence diagram outlines the signaling flow between the Guest baseband (NR), Host baseband (LTE), and the Shared Radio Unit during sharing session setup:

```mermaid
sequenceDiagram 
    participant G as Guest baseband (NR) 
    participant H as Host baseband (LTE) 
    participant RU as Shared Radio Unit 
    G->>H: Sharing session setup (capabilities, requested carriers) 
    H->>RU: Configure dual-host partition 
    RU-->>H: Partition accepted (carriers, branch mapping) 
    H-->>G: Sharing grant + measured path delay 
    G->>RU: Carrier setup via shared path (transparent through H) 
    RU-->>G: NR carriers active 
    Note over G,RU: User-plane fronthaul flows with strict priority,<br/>host restart supervision active
```

---

## Steady State and Supervision

In steady state, the sharing function acts as a transparent forwarding element with per-flow priority mapping. Supervision is continuously active in both directions:

- **Guest Supervision:** The guest node supervises the path continuity using keep-alive signaling. In the event of a path disruption, the guest node raises a specific **"shared fronthaul path failure"** alarm. This alarm is designed to point operations and maintenance (O&M) personnel directly to the host site rather than indicating a failure in the guest's own local hardware.
- **Host Supervision & Policing:** The host node supervises the guest's bandwidth usage, actively policing the guest's bandwidth allocation to protect the host's own carriers from potential misconfiguration or traffic overload.
- **User-Plane & Restart Supervision:** User-plane fronthaul flows are executed with strict priority, and host restart supervision is continuously active.

# Cross-References

- [Feature Overview](feature-overview.md)
- [Network Impact](network-impact.md)
- [Parameters](parameters.md)
- [Activation Procedure](activation-procedure.md)
