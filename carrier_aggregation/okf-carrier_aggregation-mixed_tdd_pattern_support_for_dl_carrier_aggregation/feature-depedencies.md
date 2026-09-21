---
type: concept
resource: data/vodafone-mvp/raw/Mixed TDD Pattern Support for DL Carrier Aggregation.pdf#feature-depedencies
title: Feature Dependencies
description: Details the feature, hardware, and network dependencies, as well as limitations
  for Mixed TDD Pattern Support for DL Carrier Aggregation.
tags:
- mixed-tdd
- carrier-aggregation
- dependencies
- limitations
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-11T16:39:02+00:00'
  source_sha256: ab2344bf7beef5a8
sources:
- title: Mixed TDD Pattern Support for DL Carrier Aggregation
  resource: data/vodafone-mvp/raw/Mixed TDD Pattern Support for DL Carrier Aggregation.pdf
---

The Mixed TDD Pattern Support for DL Carrier Aggregation feature extends the TDD Carrier Aggregation (CA) baseline and interacts with synchronization and coexistence functions, since TDD patterns are usually constrained by inter-operator agreements.

## Feature Dependencies

* **NR DL Carrier Aggregation:** Requires NR DL Carrier Aggregation (and the applicable multi-CC feature such as NR 3CC DL Carrier Aggregation) activated.
* **License and Control Parameters:** Requires a valid license key (`FAK-33016`) and `FeatureCtrl=MixedTddPatternCa` set to `ACTIVATED`.
* **PCell Support:** Interworks with TDD PCell Support for DL Carrier Aggregation Low/Mid-Band; the PCell may be TDD or (in EN-DC-anchored cases) FDD.
* **Slot Blanking:** Interacts with Downlink Slot Blanking for TDD Pattern Coexistence: blanked slots are excluded from the per-CC scheduling map automatically.
* **UE Capability:** UEs must support `ca-ParallelTxRx` or the applicable half-duplex behavior indication; incapable UEs are restricted to same-pattern combinations.

## Hardware Dependencies

* **Radio Units:** No radio hardware dependencies. All TDD-capable radio units are supported.
* **Baseband:** Baseband must run the extended HARQ codebook package (standard on current baseband software).

## Network Dependencies

* **Synchronization:** All aggregated TDD carriers must remain phase-synchronized to the common grid (see IEEE 1588 Time and Phase Synchronization); mixed patterns do not relax synchronization requirements.
* **Coordination:** Cross-border coordination rules for each band still apply per carrier.

## Limitations

* **Pattern Periodicity:** Maximum pattern periodicity difference is a factor of 4 between any two aggregated CCs.
* **Scheduling:** Cross-carrier scheduling between differently-patterned CCs is not supported; each mixed CC is self-scheduled.
* **Uplink CA:** UL CA across mixed patterns is not covered by this feature (downlink aggregation only; the UL remains on the PCell or same-pattern SCells).
* **Half-Duplex UEs:** Half-duplex TDD UEs cannot be configured with overlapping UL/DL conflicts; the combination builder excludes conflicting slot sets, costing up to 10% of the SCell's slots for such UEs.

# Cross-References

* [Parameters](parameters.md)
* [Activation Procedure](activation-procedure.md)
