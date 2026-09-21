---
type: reference-table
resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf#parameters
title: Parameters
description: Configuration parameters governing the Energy-Optimized Symbol Allocation
  feature.
tags:
- Energy Savings
- Symbol Allocation
- Configuration Parameters
- Radio Access Network
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:41:06+00:00'
  source_sha256: e9547a04d10e2c18
sources:
- resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf
  title: Energy-Optimized Symbol Allocation
---

This section details the configuration parameters used to control and fine-tune the Energy-Optimized Symbol Allocation feature. 

The primary guard mechanism is the load threshold (`compactionLoadThr`), above which the scheduler prioritizes frequency-domain multiplexing over symbol savings. Additionally, `minCompactedLength` is used to maintain a proportionate Demodulation Reference Signal (DMRS) overhead, as very short allocations consume a large fraction of symbols on reference signals.

### Configuration Parameters Reference

| Parameter | Description | Values | Datatype | Default |
| :--- | :--- | :--- | :--- | :--- |
| `symbolAllocMode` | Enables the function on the cell | `DISABLED`, `ENABLED` | enum | `DISABLED` |
| `compactionLoadThr` | PRB utilization above which compaction is suspended | 10–100 (%) | int32 | 30 |
| `minCompactedLength` | Minimum symbols in a compacted PDSCH | 2–7 | int32 | 4 |
| `maxCompactionUsers` | Max UEs per slot before compaction is suspended | 1–16 | int32 | 4 |
| `mappingTypeBAllowed` | Permit mapping type B for very short allocations | `false`, `true` | boolean | `true` |
| `prbWideningLimit` | Max PRB width as share of carrier bandwidth | 25–100 (%) | int32 | 100 |
| `bwpAwareCompaction` | Limit widening to the UE's active BWP | `false`, `true` | boolean | `true` |

# Cross-References

* [Feature Overview](feature-overview.md) — For information on how these parameters alter the system behavior.
* [Feature Operation](feature-operation.md) — To see how parameters like `compactionLoadThr` and `minCompactedLength` are applied during scheduler operation.
* [Activation Procedure](activation-procedure.md) — For instructions on enabling the feature via `symbolAllocMode`.
