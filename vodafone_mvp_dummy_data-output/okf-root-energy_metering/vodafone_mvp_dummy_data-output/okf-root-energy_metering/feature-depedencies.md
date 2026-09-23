---
type: concept
resource: data/vodafone-mvp/raw/Energy Metering.pdf#feature-depedencies
title: Feature Dependencies
description: Outlines feature, hardware, and network dependencies as well as limitations
  for Energy Metering.
tags:
- energy-metering
- dependencies
- hardware-dependencies
- licensing
- limitations
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T10:26:32+00:00'
  source_sha256: e85133462349aa03
sources:
- resource: data/vodafone-mvp/raw/Energy Metering.pdf
  title: Energy Metering
---

This section details the software, hardware, network dependencies, and operational limitations for the Energy Metering feature within the node architecture.

Dependencies are dominated by hardware capability: the feature reports what the installed units can measure, and falls back to model-based estimation where they cannot. Review the hardware inventory to set expectations for measurement accuracy per site before using the data in cost-allocation or regulatory reporting.

## Feature Dependencies

* Requires a valid license key (`FAK-51030`) and `FeatureCtrl=EnergyMetering` set to `ACTIVATED` under `EquipmentFunction=1`.
* Energy-saving features (NR Massive MIMO Sleep Mode, NR Booster Carrier Sleep, Radio Deep Sleep Mode, Automated Energy Saver) do not require this feature, but their measured-savings verification does.
* The bits-per-joule KPI in NR Key Performance and Resource Use Indicators uses counters from this feature as denominator.

## Hardware Dependencies

* **Integrated metering:** Requires radio and baseband hardware with DC-feed measurement circuitry (hardware generation R2 or later for radios; all current baseband units).
* **Site-level metering:** Requires a supported power distribution/battery unit connected via the site support equipment bus.
* **Units without measurement circuitry:** Covered by model-based estimation using per-product characterized power curves; these are flagged with `meteringMethod=ESTIMATED`.

## Network Dependencies

* **Measurement:** None.
* **Northbound reporting:** Uses standard PM file/streaming interfaces; no additional connectivity is required.

## Limitations

* **Manufacturing variation:** Model-based estimates do not capture unit-to-unit manufacturing variation (±5% typical error).
* **AC-side losses:** Rectifier efficiency losses are only included when a supported power system is integrated; otherwise counters represent DC consumption.
* **Counter granularity:** Sampling is continuous, but counter granularity is the 15-minute ROP; sub-ROP transients are averaged.
* **Unmanaged equipment:** External equipment on the same site power system but not managed by the node is not metered.

# Cross-References

* [Parameters](parameters.md)
* [Activation Procedure](activation-procedure.md)
* [Performance Management](performance-management.md)
