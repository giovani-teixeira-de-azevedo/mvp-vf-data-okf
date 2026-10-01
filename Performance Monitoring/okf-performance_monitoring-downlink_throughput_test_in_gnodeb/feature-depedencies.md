---
type: concept
resource: data/vodafone-mvp/raw/Downlink Throughput Test in gNodeB.pdf#feature-depedencies
title: Feature Dependencies
description: Outlines the licensing, hardware, network, and operational dependencies
  and limitations of the Downlink Throughput Test feature.
tags:
- gNodeB
- Downlink Throughput Test
- Dependencies
- Limitations
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T10:45:34+00:00'
  source_sha256: 1962030c4d1a9c52
sources:
- title: Downlink Throughput Test in gNodeB
  resource: data/vodafone-mvp/raw/Downlink Throughput Test in gNodeB.pdf
---

This section outlines the licensing, hardware, network, and operational dependencies, as well as the limitations of the Downlink Throughput Test feature in the gNodeB. The feature exercises the live scheduler and consumes real air-interface resources during a test, requiring careful consideration of licensing, target UE identification, and coexistence with other load-generating functions.

## Feature Dependencies

* **Licensing and Activation**: Requires a valid license key (`FAK-30125`) installed and the parameter `FeatureCtrl=DlThpTest` set to `ACTIVATED` under `NrFunction=1`.
* **Target-UE Selection**: Selecting a target UE by trace reference requires NR UE Trace to be active for that specific UE.
* **Coexistence**: Mutually exclusive per cell with the NR Air Interface Load Generator. A throughput test is rejected if artificial load generation is running in the same cell, as the injected load would corrupt the test results.
* **Event Export**: Test result events can be exported via Streaming of PM Events.

## Hardware Dependencies

* **Dedicated Hardware**: No dedicated hardware is required.
* **Baseband Support**: Supported on all baseband unit generations.
* **Capacity Limits**: The maximum number of simultaneous tests per node is:
  * **4** on generation B1 hardware.
  * **8** on generation B2 or later hardware.

## Network Dependencies

* **Core Network Traffic**: There are no network dependencies for the throughput measurement itself. The generated test data terminates at the UE's PDCP entity and is never forwarded to the core network. The NG-U interface carries no test traffic.
* **RRC State and Mobility**: The UE must remain in the `RRC_CONNECTED` state in the target cell for the full duration of the test. Tests are aborted on handover unless the `followOnHandover` parameter is enabled.

## Limitations

* **Resource Preemption**: Full-buffer mode (`testMode=FULL_BUFFER`) preempts other users' resources and must only be run during a maintenance window.
* **Measurement Scope**: The test measures RAN throughput only; it cannot detect transport or core network bottlenecks.
* **Inactive State**: Not supported for UEs in the `RRC_INACTIVE` state; the UE must be in `RRC_CONNECTED`.
* **Maximum Duration**: The maximum test duration is 600 seconds per invocation.
* **Massive MIMO Sleep Mode**: On cells where NR Massive MIMO Sleep Mode is active (asleep), the test forces a wake-up. This wake-up process is included in and slightly delays the first-second sample.

# Cross-References

* [Feature Overview](feature-overview.md) — For an overview of the Downlink Throughput Test feature.
* [Feature Operation](feature-operation.md) — For details on how the test is executed and managed.
* [Activation Procedure](activation-procedure.md) — For instructions on activating the license and feature control parameters.
* [Parameters](parameters.md) — For details on parameters such as `testMode` and `followOnHandover`.
* [Performance Management](performance-management.md) — For information on PM events and streaming test results.
