---
type: concept
resource: data/vodafone-mvp/raw/Fronthaul Sharing.pdf#feature-depedencies
title: Feature and Hardware Dependencies
description: Details the feature, hardware, network dependencies, and limitation requirements
  for configuring Fronthaul Sharing between host and guest basebands.
tags:
- fronthaul-sharing
- dependencies
- hardware-requirements
- limitations
- license
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:43:38+00:00'
  source_sha256: b6e9a5cf52241635
sources:
- resource: data/vodafone-mvp/raw/Fronthaul Sharing.pdf
  title: Fronthaul Sharing
---

This section outlines the software, hardware, and network dependencies, along with functional limitations, for deploying Fronthaul Sharing between a hosting baseband and a guest baseband.

Fronthaul Sharing represents a coordinated configuration contract between a hosting unit (which owns the physical fronthaul ports) and a guest unit. Successful deployment requires careful coordination of configurations across both baseband nodes, as asymmetric Virtual Local Area Network (VLAN) settings or mismatched delay budgets are common causes of integration faults.

## Feature Dependencies

* **Licensing and Activation:** Requires a valid license key (`FAK-51040`) and the parameter `FeatureCtrl=FronthaulSharing` set to `ACTIVATED` under `EquipmentFunction=1` on the hosting node. The guest node must also have the same feature activated in the guest role.
* **Carrier Sharing:** For radio-level carrier sharing between LTE and NR on the same unit, *NR Mixed Mode Radio for Massive MIMO* (for AAS radios) or *NR Combined Radio* (for classic radios) must be active.
* **Fronthaul Interworking:** eCPRI-based sharing interworks with *Long Fronthaul over eCPRI* and *Point-to-Multipoint Packet Fronthaul*. The delay budgets of all active features on a path must be summed and evaluated.
* **Multi-Operator Deployments:** This feature serves as a building block for Shared NR RAN site configurations in multi-operator deployment scenarios.

## Hardware Dependencies

* **Hosting Baseband:** Must possess fronthaul switching capabilities. All current baseband generations support this, with the earliest generation limited to CPRI cascade only.
* **Shared Radio Units:** Must support dual-host operation for radio-port sharing. This requires radio hardware generation R2 or later.
* **Fiber Plant:** Must meet the optical budget for the aggregate rate. 25G SFP28 optics are mandatory when the eCPRI flows of two basebands share a link operating above 10G aggregate rate.

## Network Dependencies

* **IP Reachability:** Host and guest basebands require mutual IP reachability to establish the sharing coordination interface. This is a lightweight control session utilizing approximately 10 kbit/s of bandwidth.
* **Synchronization:** When Time Division Duplex (TDD) carriers are shared, both basebands must be phase-synchronized to a common source (e.g., via IEEE 1588 Time and Phase Synchronization).

## Limitations

* **Segment Baseband Limit:** A maximum of two basebands can be connected per shared fronthaul segment.
* **CPRI Cascade Limit:** A maximum of two cascade hops is supported for CPRI-based sharing.
* **Impact of Host Restart:** A restart of the hosting baseband will interrupt guest fronthaul traffic passing through it. Guest cells deployed on shared paths will go down when the host restarts; joint maintenance planning is required.
* **Management Restrictions:** Guest management of shared radio hardware is strictly read-only. Hardware control actions, including restarts and firmware upgrades, must be executed as host-side operations.

# Cross-References

* [Feature Overview](feature-overview.md) — For a high-level overview of Fronthaul Sharing capabilities and roles.
* [Feature Operation](feature-operation.md) — For technical details on sharing coordination and operations.
* [Parameters](parameters.md) — For parameter details including `FeatureCtrl=FronthaulSharing`.
* [Activation Procedure](activation-procedure.md) — For step-by-step feature activation instructions.
