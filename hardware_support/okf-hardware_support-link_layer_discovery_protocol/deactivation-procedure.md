---
type: procedure
resource: data/vodafone-mvp/raw/Link Layer Discovery Protocol.pdf#deactivation-procedure
title: Deactivation Procedure
description: Detailed procedure for surgical per-port disablement or node-wide deactivation
  of the Link Layer Discovery Protocol (LLDP).
tags:
- LLDP
- RAN
- Deactivation
- CLI
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:44:39+00:00'
  source_sha256: 79b5868a69b180cf
sources:
- title: Link Layer Discovery Protocol
  resource: data/vodafone-mvp/raw/Link Layer Discovery Protocol.pdf
---

This section describes the procedure for disabling or deactivating the Link Layer Discovery Protocol (LLDP) on the node, either surgically per port or node-wide.

## Impact Analysis

*   **Traffic Impact:** None. Disabling LLDP stops advertisements and clears local neighbor tables, but no traffic-carrying function is affected.
*   **Neighbor Behavior:** Peer nodes age the node out of their tables within the Time To Live (TTL) period, or immediately via the shutdown advertisement.
*   **OSS Coordination:** Any OSS cabling-verification workflow will flag the node as "topology unknown" after deactivation. These checks should be suppressed first to avoid false alarms.

## Deactivation Methods

Consider per-port disablement (Step 1) instead of full deactivation where only specific segments must stop advertising. Full deactivation (Step 2) blinds topology tooling for the entire node.

### Step 1: Disable LLDP on a Specific Port (Preferred, Surgical)

Execute the following CLI command to disable LLDP on a specific port (e.g., `TN-B`):

```bash
rancli set NodeRoot=1,TransportFunction=1,EthPort=TN-B adminStatus=DISABLED
```

### Step 2: Deactivate the Feature Node-Wide (Alternative)

Execute the following CLI command to deactivate LLDP across the entire node:

```bash
rancli set NodeRoot=1,TransportFunction=1,FeatureCtrl=Lldp featureState=DEACTIVATED
```

### Step 3: Verification

Execute the following CLI command to verify the port status and the neighbor table:

```bash
rancli get NodeRoot=1,TransportFunction=1,EthPort=TN-A adminStatus lldpNeighborTable
```

# Cross-References

*   [Activation Procedure](activation-procedure.md) - For enabling LLDP on the node.
*   [Network Impact](network-impact.md) - Detailed analysis of how LLDP behaves in the network.
*   [Parameters](parameters.md) - Configuration parameters including `adminStatus` and `featureState`.
