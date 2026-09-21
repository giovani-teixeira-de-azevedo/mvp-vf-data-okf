---
type: procedure
resource: data/vodafone-mvp/raw/Link Layer Discovery Protocol.pdf#activation-procedure
title: Activation Procedure
description: Step-by-step procedure for activating LLDP, configuring port policies,
  and verifying neighbor discovery.
tags:
- lldp
- activation
- configuration
- cli
- transport
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:44:26+00:00'
  source_sha256: 98af66475d7315d2
sources:
- resource: data/vodafone-mvp/raw/Link Layer Discovery Protocol.pdf
  title: Link Layer Discovery Protocol
---

This section describes the step-by-step procedure to activate and configure the Link Layer Discovery Protocol (LLDP) feature, including port-specific configurations and operational verification.

## Traffic Impact and Preconditions

### Traffic Impact
There is **no traffic impact**. LLDP is a low-rate control protocol operating at the link layer. Enabling it has no effect on user traffic, transport routing, or radio operation. It can be safely activated fleet-wide during normal business hours.

### Preconditions
* **License Requirements:** None beyond base software; the feature is unlicensed and does not require a license key.
* **Preparation:** Decide the per-port policy prior to execution:
  * **Bidirectional (`TX_AND_RX`):** For trusted or owned transport.
  * **Receive-only (`RX_ONLY`):** Towards third-party equipment.
* **Recommended Rollout Strategy:** Fleet-wide implementation with default timers, followed by an OSS topology reconciliation run to harvest the first full neighbor snapshot.

---

## Step-by-Step Procedure

The activation procedure is executed via CLI using the following four steps:

### Step 1: Activate the Feature
Activate LLDP globally in the base package. No license key is required.

```bash
rancli set NodeRoot=1,TransportFunction=1,FeatureCtrl=Lldp featureState=ACTIVATED
```

### Step 2: Enable Bidirectional LLDP on the Backhaul Port
Configure the backhaul port (e.g., `TN-A`) to transmit and receive LLDP frames, and set the transmit interval.

```bash
rancli set NodeRoot=1,TransportFunction=1,EthPort=TN-A adminStatus=TX_AND_RX txInterval=30
```

### Step 3: Configure Receive-Only toward Third-Party Equipment
Set the facing port (e.g., `TN-B`) connected to third-party networks to receive-only mode.

```bash
rancli set NodeRoot=1,TransportFunction=1,EthPort=TN-B adminStatus=RX_ONLY
```

### Step 4: Verify Neighbor Discovery
Query the LLDP neighbor table to verify that adjacent devices are discovered. 

```bash
rancli get NodeRoot=1,TransportFunction=1,EthPort=TN-A lldpNeighborTable
```

**Expected Result:** 
The command should return the `chassisId`, `portId`, and `systemName` of the connected router. Note that the far-end chassis ID and port ID should appear within one `txInterval` of the peer.

## Cross-References

* [Feature Overview](feature-overview.md)
* [Parameters](parameters.md)
* [Deactivation Procedure](deactivation-procedure.md)
* [Network Impact](network-impact.md)
