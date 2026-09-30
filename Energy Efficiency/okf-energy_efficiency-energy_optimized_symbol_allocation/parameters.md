---
type: reference-table
resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf#parameters
title: Parameters
description: Configuration parameters for the Energy-Optimized Symbol Allocation feature,
  including load thresholds, compaction lengths, and bandwidth limits.
tags:
- parameters
- configuration
- symbol-allocation
- energy-saving
- compaction
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-30T17:15:58+00:00'
  source_sha256: e9547a04d10e2c18
sources:
- resource: data/vodafone-mvp/raw/Energy-Optimized Symbol Allocation.pdf
  title: Energy-Optimized Symbol Allocation
---

This section details the configuration parameters for the Energy-Optimized Symbol Allocation feature.

The load threshold is the primary guard: above it, the scheduler needs frequency-domain freedom for multiplexing more than it needs symbol savings. `minCompactedLength` exists to keep DMRS overhead proportionate — very short allocations spend a large fraction of their symbols on reference signals.

## Parameter List

| Parameter | Description | Values | Datatype | Default |
| --- | --- | --- | --- | --- |
| `symbolAllocMode` | Enables the function on the cell | DISABLED, ENABLED | enum | DISABLED |
| `compactionLoadThr` | PRB utilization above which compaction is suspended | 10–100 (%) | int32 | 30 |
| `minCompactedLength` | Minimum symbols in a compacted PDSCH | 2–7 | int32 | 4 |
| `maxCompactionUsers` | Max UEs per slot before compaction is suspended | 1–16 | int32 | 4 |
| `mappingTypeBAllowed` | Permit mapping type B for very short allocations | false, true | boolean | true |
| `prbWideningLimit` | Max PRB width as share of carrier bandwidth | 25–100 (%) | int32 | 100 |
| `bwpAwareCompaction` | Limit widening to the UE's active BWP | false, true | boolean | true |

# Cross-References

- [FEATURE OVERVIEW](feature-overview.md)
- [FEATURE OPERATION](feature-operation.md)
- [ACTIVATION PROCEDURE](activation-procedure.md)
- [DEACTIVATION PROCEDURE](deactivation-procedure.md)
- [FEATURE DEPENDENCIES](feature-depedencies.md)
- [NETWORK IMPACT](network-impact.md)
- [PERFORMANCE MANAGEMENT](performance-management.md)
