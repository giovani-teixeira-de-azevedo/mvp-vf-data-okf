---
type: reference-table
resource: data/vodafone-mvp/raw/Downlink Throughput Test in gNodeB.pdf#parameters
title: Parameters
description: Node-wide test policy parameters for configuring the Downlink Throughput
  Test in gNodeB.
tags:
- gNodeB
- Downlink Throughput
- Parameters
- Configuration
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T10:45:38+00:00'
  source_sha256: ca586810af363936
sources:
- title: Downlink Throughput Test in gNodeB
  resource: data/vodafone-mvp/raw/Downlink Throughput Test in gNodeB.pdf
---

This section details the configuration parameters that define the node-wide test policy for the Downlink Throughput Test in the gNodeB. These parameters are configured under the Managed Object (MO) path `NodeRoot=1,NrFunction=1,DlThpTest=1`.

## Node-Wide Test Policy Parameters

The parameters below configure the global boundaries and thresholds for the downlink throughput test. Per-test values (such as cell, UE, duration, and mode) are supplied as action arguments during test execution. The default values are selected to ensure that an accidentally initiated test does not disrupt live traffic beyond a bounded share.

| Parameter | Description | Values | Datatype | Default |
| :--- | :--- | :--- | :--- | :--- |
| `testModeAllowed` | Highest test mode an operator action may request | `RESOURCE_CAPPED`, `FULL_BUFFER` | enum | `RESOURCE_CAPPED` |
| `maxTestPrbShare` | Maximum Physical Resource Block (PRB) share grantable to a capped test | 5–50 (%) | int32 | 20 |
| `maxTestDuration` | Upper bound on requested test duration | 10–600 (s) | int32 | 120 |
| `maxParallelTests` | Maximum simultaneous tests per node | 1–8 | int32 | 2 |
| `followOnHandover` | Continue the test after intra-node handover | `true`, `false` | boolean | `false` |
| `passThresholdRatio` | Measured/expected throughput ratio for PASSED verdict | 50–100 (%) | int32 | 80 |
| `degradedThresholdRatio` | Ratio below which verdict is FAILED | 10–90 (%) | int32 | 50 |
| `resultRetention` | Number of test records retained on the node | 1–100 | int32 | 20 |

# Cross-References

* [Feature Operation](feature-operation.md) — Details how these parameters are applied during test execution and how action arguments are supplied.
* [Activation Procedure](activation-procedure.md) — Describes the steps to activate and configure the Downlink Throughput Test feature.
