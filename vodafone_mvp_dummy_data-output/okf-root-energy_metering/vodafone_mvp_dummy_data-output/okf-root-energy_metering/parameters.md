---
type: reference-table
resource: data/vodafone-mvp/raw/Energy Metering.pdf#parameters
title: PARAMETERS
description: Configuration parameters for the Energy Metering feature, controlling
  polling cadence, estimation fallback, and high-consumption alarms.
tags:
- parameters
- configuration
- energy-metering
- polling-interval
- high-consumption-alarm
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T10:26:28+00:00'
  source_sha256: eece738063db2505
sources:
- title: Energy Metering
  resource: data/vodafone-mvp/raw/Energy Metering.pdf
---

This section outlines the configuration parameters for the Energy Metering feature.

Configuration is light: the feature is largely plug-and-measure. The parameters control polling cadence, estimation fallback, and the optional high-consumption alarm, which is the one setting worth tuning per site class (a small indoor site and a 64T64R macro differ by an order of magnitude in normal draw).

## Parameter List

| Parameter | Description | Values | Datatype | Default |
| --- | --- | --- | --- | --- |
| `meteringEnabled` | Enables energy accumulation per unit | true, false | boolean | true |
| `samplingInterval` | Internal poll interval of unit meters | 1–60 (s) | int32 | 1 |
| `estimationFallback` | Use power-model estimates for unmetered units | true, false | boolean | true |
| `siteAggregation` | Include site power system delta in node total | true, false | boolean | true |
| `highConsumptionThr` | Node-level alarm threshold for average power | 100–20000 (W) | int32 | 10000 |
| `highConsumptionTimer` | Time above threshold before alarm | 60–3600 (s) | int32 | 900 |
| `reportEstimatedSeparately` | Split measured vs estimated energy in counters | true, false | boolean | true |
