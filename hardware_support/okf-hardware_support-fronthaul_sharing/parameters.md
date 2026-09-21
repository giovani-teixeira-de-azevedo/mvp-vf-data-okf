---
type: reference-table
resource: data/vodafone-mvp/raw/Fronthaul Sharing.pdf#parameters
title: Parameters
description: Configuration parameters for host and guest nodes in Fronthaul Sharing,
  including bandwidth partition and delay budget checks.
tags:
- fronthaul-sharing
- parameters
- configuration
- telecom
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:44:05+00:00'
  source_sha256: a72c03748e30cc7f
sources:
- title: Fronthaul Sharing
  resource: data/vodafone-mvp/raw/Fronthaul Sharing.pdf
---

This section details the configuration parameters used to manage Fronthaul Sharing on both host and guest nodes. These parameters control bandwidth partitioning, delay checks, port configurations, and path supervision settings.

## Parameters Overview

Fronthaul sharing parameters exist on both the host and guest sides of the segment. 

The two most critical parameters are:
* **`guestMaxBandwidth`** (bandwidth partition)
* **`maxSharedPathDelay`** (delay budget check)

The node automatically refuses any carrier setup that would violate either of these parameters. This mechanism turns potential dimensioning errors into explicit configuration rejections rather than letting them manifest as subtle, hard-to-diagnose air-interface faults.

## Parameter List

| Parameter | Description | Values | Datatype | Default |
| :--- | :--- | :--- | :--- | :--- |
| `sharingRole` | Role of this node on the segment | HOST, GUEST, NONE | enum | NONE |
| `guestNodeId` | Identity of the peer guest node (host side) | string | string | "" |
| `guestMaxBandwidth` | Fronthaul capacity reserved for guest flows | 1–25 (Gbit/s) | int32 | 10 |
| `maxSharedPathDelay` | Max accepted one-way path delay | 5–100 (µs) | int32 | 30 |
| `sharedPortList` | Fronthaul ports marked as shared | list of port ids | string | "" |
| `keepAliveInterval` | Path supervision keep-alive period | 100–5000 (ms) | int32 | 1000 |
| `pathFailureTimer` | Keep-alive losses before path-failure alarm | 3–20 | int32 | 5 |
| `guestPolicingMode` | Enforcement on guest bandwidth overuse | DROP, ALARM_ONLY | enum | DROP |

# Cross-References

* [Feature Overview](feature-overview.md) — For a high-level view of the fronthaul sharing feature.
* [Feature Operation](feature-operation.md) — For how these parameters function during real-time network operations.
* [Activation Procedure](activation-procedure.md) — For details on configuring these parameters during feature deployment.
