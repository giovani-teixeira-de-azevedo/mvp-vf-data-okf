---
type: concept
resource: data/vodafone-mvp/raw/Long Fronthaul over eCPRI.pdf#feature-depedencies
title: Feature Dependencies
description: Outlines the licensing, hardware, network, and operational dependencies
  and limitations for the Long Fronthaul over eCPRI feature.
tags:
- eCPRI
- Fronthaul
- Dependencies
- Hardware Requirements
- Network Requirements
- Limitations
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:45:05+00:00'
  source_sha256: 117606925af6a05c
sources:
- title: Long Fronthaul over eCPRI
  resource: data/vodafone-mvp/raw/Long Fronthaul over eCPRI.pdf
---

This section outlines the transport quality, synchronization, hardware, network, and license dependencies, as well as the operational limitations for the Long Fronthaul over eCPRI feature. Because long fronthaul distances (up to 40 km) involve metro transport networks rather than direct patch cabling, validating these delay, jitter, and asymmetry characteristics prior to licensing and configuration is critical.

## Key Considerations

At distances of 40 km, the fronthaul behaves as a metro network. The delay and jitter characteristics of this network directly impact the radio configuration. It is essential to validate the transport design before beginning licensing discussions, as an unsuitable transport network cannot be compensated for through configuration.

## Feature Dependencies

* **Licensing and Control:**
  * Requires a valid license key (**FAK-51050**).
  * Requires the parameter `FeatureCtrl=LongFronthaulEcpri` to be set to `ACTIVATED` under `EquipmentFunction=1`.
* **Fronthaul Protocol:**
  * Requires eCPRI-based fronthaul.
  * CPRI-based radios are excluded (CPRI long-haul is not supported by this feature).
* **Interworking:**
  * Interworks with *Point-to-Multipoint Packet Fronthaul* (shared fronthaul aggregation) and *Fronthaul Sharing*.
  * The delay budgets compose, and the validation step checks the sum of these budgets.
* **Synchronization:**
  * Time synchronization per IEEE 1588 Time and Phase Synchronization is required at the radio site when the synchronization plane (sync plane) traverses the fronthaul.
  * Alternatively, radios must use a local GNSS receiver (Clock Source over GPS).

## Hardware Dependencies

* **Baseband Units:** Must have eCPRI fronthaul ports operating at 10/25 Gbit/s with long-reach optics (ER/ZR class, or foreign wavelengths over DWDM) matching the physical fiber plan.
* **Radio Units:** Must be of hardware generation **R2 or later** to support the extended buffer memory needed for delay compensation.
* **Fronthaul Switches:** If present in the transport path, they must support **ITU-T G.8273.2 Class C** timing or better when carrying the sync plane.

## Network Dependencies

* **Fronthaul Delay:**
  * One-way fronthaul delay must be $\le 200\ \mu\text{s}$ (approximately 40 km) for 30 kHz subcarrier spacing (SCS).
  * One-way delay must be $\le 100\ \mu\text{s}$ for configurations requiring short HARQ loops (such as low-latency 5QI profiles).
* **Packet Delay Variation (PDV):** Must be $\le 5\ \mu\text{s}$ peak-to-peak on the fronthaul path.
* **Fiber Asymmetry:** Transmit-to-receive fiber path asymmetry must be $\le 1\ \mu\text{s}$, as asymmetry directly affects path timing accuracy.

## Limitations

* **Distance and SCS Limits:** 
  * Maximum one-way delay of $200\ \mu\text{s}$ (40 km) at 30 kHz SCS.
  * High-band (120 kHz SCS) configurations are strictly limited to a maximum of 20 km.
* **User Latency:** UE-perceived latency increases by twice the one-way fronthaul delay, resulting in an overhead of up to approximately 0.4 ms at maximum reach.
* **Throughput Overhead:** Due to the increased round-trip time (RTT) in the scheduler's outstanding-data window, the HARQ round-trip extension marginally reduces peak throughput at maximum distance (up to a 3% reduction at 40 km).
* **Incompatibilities:** Not supported in combination with cell configurations that utilize ultra-short processing time budgets.

# Cross-References

* [Feature Overview](feature-overview.md) — For information on the capability and architecture of Long Fronthaul over eCPRI.
* [Feature Operation](feature-operation.md) — For details on delay calculations and transport validation.
* [Parameters](parameters.md) — For details on the configuration parameters like `FeatureCtrl`.
* [Activation Procedure](activation-procedure.md) — For step-by-step instructions on enabling this feature.
