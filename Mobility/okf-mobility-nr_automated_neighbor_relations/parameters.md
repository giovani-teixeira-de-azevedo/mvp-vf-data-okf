---
type: reference-table
resource: data/vodafone-mvp/raw/NR Automated Neighbor Relations.pdf#parameters
title: Parameters
description: Configuration parameters for NR Automated Neighbor Relations (ANR) at
  the NR function and cell levels.
tags:
- parameters
- anr
- configuration
- nr-ran
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T16:57:11+00:00'
  source_sha256: f408bded651b7de7
sources:
- title: NR Automated Neighbor Relations
  resource: data/vodafone-mvp/raw/NR Automated Neighbor Relations.pdf
---

This section provides the configuration parameters for the NR Automated Neighbor Relations (ANR) feature. Parameters are configured at the NR function level, with support for per-cell overrides where specified.

While default values are designed to suit most network deployments, the relation removal timer (`relationRemovalTime`) is commonly tuned (e.g., set to a longer duration in networks with seasonal traffic patterns to ensure winter-only relations survive during summer periods).

## Parameter Reference

| Parameter | Description | Values | Datatype | Default |
| :--- | :--- | :--- | :--- | :--- |
| `anrState` | Enables ANR (per cell override supported) | `DISABLED`, `INTRA_FREQ_ONLY`, `ALL` | enum | `ALL` |
| `autoXnSetup` | Automatically establish Xn toward discovered gNodeBs | `true`, `false` | boolean | `true` |
| `interRatAnrEnabled` | Enables E-UTRAN relation discovery | `true`, `false` | boolean | `true` |
| `maxCgiOrdersPerCell` | Concurrent CGI report orders per cell | `1–16` | int32 | `4` |
| `cgiReportTimer` | Supervision timer per CGI order | `2–16 (s)` | int32 | `8` |
| `relationRemovalTime` | Days without handover attempts before auto-removal | `1–90 (days)` | int32 | `30` |
| `maxNeighborRelations` | Cap on relations per cell | `64–1024` | int32 | `512` |
| `anrHoFailAlarmThr` | Failure ratio raising the chronic-failure alarm | `10–90 (%)` | int32 | `50` |
