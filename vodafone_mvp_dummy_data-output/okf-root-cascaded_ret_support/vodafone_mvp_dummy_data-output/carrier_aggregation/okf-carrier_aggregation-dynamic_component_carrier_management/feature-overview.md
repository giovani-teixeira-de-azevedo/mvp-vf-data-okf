---
type: concept
resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf#feature-overview
title: FEATURE OVERVIEW
description: Overview of Dynamic Component Carrier Management, providing automated,
  load-driven selection, scoring, and redistribution of component carriers for CA-capable
  UEs.
tags:
- dynamic-component-carrier-management
- carrier-aggregation
- scell
- load-balancing
- prb-utilization
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T15:05:07+00:00'
  source_sha256: 6820c794e28a30b9
sources:
- title: Dynamic Component Carrier Management
  resource: data/vodafone-mvp/raw/Dynamic Component Carrier Management.pdf
---

This section provides an overview of the Dynamic Component Carrier Management feature within the overall document structure.

## Feature Summary

Dynamic Component Carrier Management automates the selection, prioritization, and load-driven redistribution of component carriers across the Carrier Aggregation (CA) capable User Equipment (UE) population of a node. 

In a statically configured network, each cell carries an operator-defined Secondary Cell (SCell) candidate list with fixed priorities, steering every CA-capable UE toward the same carriers in the same order. Static priorities cannot adapt when traffic load shifts across carriers (for example, a mid-band capacity layer congested at peak hours while an adjacent low-band carrier is underutilized), requiring operators to either over-dimension capacity or accept unbalanced carrier loading.

This feature replaces static candidate lists with dynamic, per-UE carrier assignments calculated at SCell setup time and re-evaluated periodically.

## Dynamic Carrier Assignment and Scoring

The carrier manager gathers per-carrier load metrics and combines them with per-UE inputs to compute a composite score for each candidate carrier. SCells are then configured in order of score.

* **Per-Carrier Load Metrics:**
  * Physical Resource Block (PRB) utilization
  * Active UE count
  * Physical Downlink Control Channel (PDCCH) utilization
  * Average achieved throughput per scheduled UE
* **Per-UE Inputs:**
  * Band-combination capability
  * Measured Reference Signal Received Power (RSRP) per candidate carrier
  * Current traffic demand

## Background Load Rebalancing

A background rebalancing process actively manages long-lived high-volume UEs (typically Fixed Wireless Access / FWA):
* Identifies carriers crossing a sustained-load threshold (`rebalanceThr`).
* Migrates SCells of high-volume UEs away from heavily loaded carriers.
* Uses SCell release/add reconfigurations paced to prevent signaling bursts.

## Observed Performance Impact

Operators typically observe the following performance improvements on multi-carrier sites:
* **10–25% higher aggregate downlink throughput** during busy hours.
* **Visible reduction in load spread** across carriers, with the standard deviation of PRB utilization across carriers commonly halving.
* **Reduction in congestion-related scheduling delays** on the previously default-first carrier.

## Feature Workflow

```mermaid
flowchart TD 
    A[SCell setup or periodic re-evaluation] --> B[Collect per-carrier load metrics] 
    B --> C[Filter by UE band combination + RSRP] 
    C --> D[Score = w1·load + w2·RSRP + w3·throughput] 
    D --> E[Configure SCells in score order] 
    E --> F{Carrier above rebalanceThr?} 
    F -- yes --> G[Migrate paced subset of SCell UEs] 
    F -- no --> A 
    G --> A
```
