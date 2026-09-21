---
type: concept
resource: data/vodafone-mvp/raw/Energy Metering.pdf#feature-depedencies
title: Feature Dependencies
description: Hardware, software, network, and operational dependencies and limitations
  for the Energy Metering feature.
tags:
- energy-metering
- dependencies
- hardware-requirements
- limitations
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:43:09+00:00'
  source_sha256: e85133462349aa03
sources:
- resource: data/vodafone-mvp/raw/Energy Metering.pdf
  title: Energy Metering
---

This section describes the hardware, software, network, and operational dependencies, as well as the technical limitations of the Energy Metering feature. Dependencies are primarily governed by the hardware capability of the installed units, falling back to model-based estimation where hardware measurement is not supported.

## Feature Dependencies

* **License & Activation**: Requires a valid license key (`FAK-51030`) and the parameter `FeatureCtrl=EnergyMetering` set to `ACTIVATED` under `EquipmentFunction=1`.
* **Coexistence with Energy-Saving Features**: Energy-saving features (such as NR Massive MIMO Sleep Mode, NR Booster Carrier Sleep, Radio Deep Sleep Mode, and Automated Energy Saver) do not require Energy Metering to function. However, verifying the measured savings of those features does depend on the counters provided by this feature.
* **KPI Integration**: The bits-per-joule KPI in the *NR Key Performance and Resource Use Indicators* uses counters from this feature as its denominator.

## Hardware Dependencies

* **Integrated Metering**: Requires radio and baseband hardware equipped with DC-feed measurement circuitry. This corresponds to hardware generation R2 or later for radios, and all current baseband units.
* **Site-Level Metering**: Requires a supported power distribution or battery unit connected via the site support equipment bus.
* **Estimation Fallback**: Units lacking physical measurement circuitry are covered by model-based estimation using per-product characterized power curves. These estimation-based measurements are flagged with `meteringMethod=ESTIMATED`.

## Network Dependencies

* **Measurement**: No specific network dependencies for performing measurements.
* **Reporting**: Northbound reporting utilizes standard Performance Management (PM) file and streaming interfaces. No additional network connectivity or specialized protocols are required.

## Technical Limitations

* **Manufacturing Variation**: Model-based estimates do not capture unit-to-unit manufacturing variation, which typically introduces an error of ±5%.
* **AC-Side Losses**: AC-side losses (such as rectifier efficiency) are only included when a supported, integrated power system is present. Otherwise, the reported counters represent DC consumption only.
* **Granularity and Sampling**: Although sampling is continuous, counter granularity is fixed at the 15-minute Reporting Period (ROP). Sub-ROP transients are averaged out over the period.
* **Unmanaged External Equipment**: External equipment installed on the same site power system that is not managed by the node is not metered.

# Cross-References

* [Feature Overview](feature-overview.md) — For an overview of the metering methods and architecture.
* [Activation Procedure](activation-procedure.md) — For step-by-step instructions on enabling the feature license and parameters.
* [Parameters](parameters.md) — For detailed configurations, including `FeatureCtrl` and `meteringMethod`.
* [Performance Management](performance-management.md) — For description of counters and performance reporting.
