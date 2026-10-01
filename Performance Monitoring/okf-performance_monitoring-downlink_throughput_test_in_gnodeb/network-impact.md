---
type: concept
resource: data/vodafone-mvp/raw/Downlink Throughput Test in gNodeB.pdf#network-impact
title: Network Impact
description: Analyzes the impact of the Downlink Throughput Test on end users, signaling,
  transport/core networks, and performance KPIs.
tags:
- downlink-throughput
- network-impact
- gnodeb
- kpi
- resource-capped
- full-buffer
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T10:45:28+00:00'
  source_sha256: b245824d6c4aa170
sources:
- title: Downlink Throughput Test in gNodeB
  resource: data/vodafone-mvp/raw/Downlink Throughput Test in gNodeB.pdf
---

This section describes the network impact of running the Downlink Throughput Test in the gNodeB, detailing its effects on end users, signaling, transport/core networks, and performance key performance indicators (KPIs).

### Impact Analysis

*   **End Users**:
    *   **RESOURCE_CAPPED Mode**: Other users in the cell lose at most `maxTestPrbShare` (default 20%) of Physical Resource Blocks (PRBs) for the duration of the test. Guaranteed Bit Rate (GBR) bearers are never preempted.
    *   **FULL_BUFFER Mode**: Best-effort users in the cell can experience starvation. Consequently, running the test in this mode requires a maintenance window.
*   **Signaling**: The signaling impact is negligible. The test does not add any Radio Resource Control (RRC) procedures, with the exception of optional measurement configurations.
*   **Transport/Core**: There is no impact on the transport or core network, as the test data is generated and terminated entirely within the Radio Access Network (RAN).
*   **KPIs**: Cell downlink volume and PRB utilization counters include the test traffic. To prevent distortion of capacity trending, recurring scheduled tests should be excluded from capacity planning. The test volume is separately counted in the counter `ctrTestDataVolume` to enable this exclusion.

# Cross-References

*   [Feature Operation](feature-operation.md) — Details the test modes including `RESOURCE_CAPPED` and `FULL_BUFFER`.
*   [Parameters](parameters.md) — Defines parameters such as `maxTestPrbShare`.
*   [Performance Management](performance-management.md) — Describes performance counters including `ctrTestDataVolume`.
