---
type: concept
resource: data/vodafone-mvp/raw/Data-Aware Carrier Management.pdf#feature-overview
title: FEATURE OVERVIEW
description: Overview of Data-Aware Carrier Management, a gNodeB feature that dynamically
  configures and activates NR carrier aggregation SCells based on per-UE data demand
  classification.
tags:
- carrier-aggregation
- scell
- nr
- gnodeb
- demand-estimator
- data-aware-carrier-management
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T13:48:18+00:00'
  source_sha256: 7047de60e9ef1300
sources:
- title: Data-Aware Carrier Management
  resource: data/vodafone-mvp/raw/Data-Aware Carrier Management.pdf
---

This section provides an overview of the Data-Aware Carrier Management feature for NR carrier aggregation in the gNodeB.

## Overview

Data-Aware Carrier Management makes secondary cell (SCell) configuration and activation decisions in NR carrier aggregation dependent on the actual and predicted data demand of each UE, instead of configuring SCells statically for every CA-capable UE at connection setup. 

In a conventional CA implementation, a CA-capable UE entering `RRC_CONNECTED` is immediately configured with the full set of candidate SCells, and those SCells are activated on a simple buffer threshold. While this maximizes peak throughput readiness, it introduces inefficiencies:
* A UE exchanging keep-alive traffic or a short web transaction incurs SCell measurement configuration costs, activation delay, and increased UE battery drain.
* The gNodeB spends Physical Downlink Control Channel (PDCCH) capacity and RRC signaling on carriers the UE will never fill.

## Per-UE Demand Estimator and Classification

The feature introduces a per-UE demand estimator within the gNodeB that observes:
* Downlink and uplink buffer dynamics
* Historical burst sizes
* PDU session 5QI mix
* Short-term throughput

Based on these inputs, each UE is classified into demand classes:

| Demand Class | SCell Configuration and Activation Behavior |
| :--- | :--- |
| **BACKGROUND** | UE remains on the PCell only (no SCell configuration). |
| **INTERACTIVE** | UE is configured with a single fast-activating SCell (deactivated). |
| **BULK** | Immediate configuration and activation of all applicable SCells. |

The classification is re-evaluated continuously. If a UE starts a large download, it is promoted within tens of milliseconds using MAC Control Element (MAC CE)-based SCell activation as per 3GPP TS 38.321.

## Key Benefits and Net Effects

* **30–50% fewer SCell activations** network-wide.
* **Negligible loss of user throughput** (typically under 2% at the 90th percentile).
* **Reduced RRC reconfiguration volume**.
* **Lower UE energy consumption**.
* **Freed PDCCH and CSI resources** on SCell carriers, allocating capacity to UEs that require it.

## Decision Flow Diagram

```mermaid
flowchart TD 
    A[UE enters RRC_CONNECTED] --> B[Demand estimator: buffer, 5QI, history] 
    B --> C{Demand class} 
    C -- BACKGROUND --> D[PCell only, no SCell config] 
    C -- INTERACTIVE --> E[Configure 1 SCell, deactivated] 
    C -- BULK --> F[Configure all SCells, activate via MAC CE] 
    D --> B 
    E --> B 
    F --> G[Data burst served on aggregated carriers] 
    G --> B
```

# Cross-References

* [FEATURE OPERATION](feature-operation.md)
* [ACTIVATION PROCEDURE](activation-procedure.md)
* [DEACTIVATION PROCEDURE](deactivation-procedure.md)
* [NETWORK IMPACT](network-impact.md)
