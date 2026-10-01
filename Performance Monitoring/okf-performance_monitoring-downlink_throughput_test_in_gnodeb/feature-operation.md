---
type: concept
resource: data/vodafone-mvp/raw/Downlink Throughput Test in gNodeB.pdf#feature-operation
title: Feature Operation
description: Describes the execution modes, sampling metrics, and termination behavior
  of the Downlink Throughput Test in gNodeB.
tags:
- gNodeB
- Downlink Throughput Test
- Resource Capped Mode
- Full Buffer Mode
- Throughput Testing
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T10:45:29+00:00'
  source_sha256: 7ed883ffb32fcfee
sources:
- title: Downlink Throughput Test in gNodeB
  resource: data/vodafone-mvp/raw/Downlink Throughput Test in gNodeB.pdf
---

This section describes the operational mechanics of the Downlink Throughput Test in the gNodeB, including test invocation, execution modes, sampled metrics, and test termination behavior.

## Test Invocation and Modes

A test is invoked via a Managed Object (MO) action, which specifies:
* The target cell
* The target UE
* The test duration
* The test mode

The test supports two execution modes:

* **`RESOURCE_CAPPED` mode (default)**: The test controller registers a padding-data flow with the scheduler. This flow is served only up to `maxTestPrbShare` of the cell's Physical Resource Blocks (PRBs) and always yields to guaranteed-bit-rate (GBR) traffic. This makes daytime testing safe without impacting user experience.
* **`FULL_BUFFER` mode**: The padding-data flow is scheduled like a maximum-priority full-buffer UE, delivering the true peak capability of the radio link.

## Sampling and Metrics

During the test window, the test controller samples the following metrics once per second:
* Scheduled PDCP volume
* MAC volume (including retransmissions)
* Average and distributional Modulation and Coding Scheme (MCS)
* Reported Channel Quality Indicator (CQI)
* Transmission rank
* Initial-transmission Block Error Rate (BLER)
* PRB share actually granted

## Test Termination and Verdict

The test ends upon duration expiry, operator stop, or abort on UE release. When the test terminates:
* The collected samples are assembled into a test record written to the Performance Management (PM) event stream.
* The test record is retrievable via the `lastTestResult` attribute.
* A summary verdict is computed by comparing the measured PDCP throughput against an expected value. This expected value is calculated based on:
  * Cell bandwidth
  * UE's reported capability
  * Observed radio quality
* The test is flagged with one of the following summary verdicts:
  * `PASSED`
  * `DEGRADED`
  * `FAILED`

# Cross-References

* [Feature Overview](feature-overview.md)
* [Parameters](parameters.md) (for `maxTestPrbShare` and other configuration parameters)
* [Performance Management](performance-management.md) (for PM event stream and `lastTestResult` details)
