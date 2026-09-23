---
type: reference-table
resource: data/vodafone-mvp/raw/Data-Aware Carrier Management.pdf#parameters
title: Parameters
description: Configuration parameters for Data-Aware Carrier Management including
  ranges, datatypes, and default values.
tags:
- parameters
- configuration
- nrfunction
- carrier-management
- burst-thresholds
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-22T13:53:35+00:00'
  source_sha256: 16c6db4c0eeb7a3c
sources:
- resource: data/vodafone-mvp/raw/Data-Aware Carrier Management.pdf
  title: Data-Aware Carrier Management
---

Parameters are set on `NrFunction=1` (node scope) with per-cell override of the burst thresholds. Defaults are tuned for mixed smartphone traffic; FWA-dominated cells should raise `bulkBurstThr` since almost all FWA traffic qualifies as BULK anyway.

| Parameter | Description | Values | Datatype | Default |
| --- | --- | --- | --- | --- |
| `dataAwareCmEnabled` | Enables demand-based carrier management | true, false | boolean | false |
| `bulkBurstThr` | Burst size classifying a UE as BULK | 100–100000 (kB) | int32 | 500 |
| `interactiveBurstThr` | Burst size classifying a UE as INTERACTIVE | 10–10000 (kB) | int32 | 50 |
| `classDwellTimer` | Minimum time in a class before demotion | 100–10000 (ms) | int32 | 1000 |
| `demoteTimer` | Idle time before SCell deactivation | 100–60000 (ms) | int32 | 5000 |
| `deconfigTimer` | Further idle time before SCell de-configuration | 1000–300000 (ms) | int32 | 30000 |
| `voiceUeMinClass` | Minimum class for UEs with 5QI-1 bearers | INTERACTIVE, BULK | enum | INTERACTIVE |
| `historyDepth` | Number of bursts kept in the per-UE history | 4–64 | int32 | 16 |
