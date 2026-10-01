---
type: concept
resource: data/vodafone-mvp/raw/NR Data-Aware Mobility.pdf#feature-depedencies
title: Feature Dependencies
description: Outlines the feature, hardware, and network dependencies, as well as
  the operational limitations of the NR Data-Aware Mobility feature.
tags:
- dependencies
- limitations
- hardware-dependencies
- network-dependencies
- nr-data-aware-mobility
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T10:02:22+00:00'
  source_sha256: ad768ecaecaef5d0
sources:
- resource: data/vodafone-mvp/raw/NR Data-Aware Mobility.pdf
  title: NR Data-Aware Mobility
---

This section outlines the feature, hardware, and network dependencies, as well as the operational limitations of the NR Data-Aware Mobility feature. The feature is a decision-layer enhancement on top of the baseline mobility engine, and its target-scoring leg depends on inter-node load information.

### Feature Dependencies

* **NR Mobility**: Requires NR Mobility to be activated; this feature modulates its triggers and target selection.
* **License and Control**: Requires a valid license key (`FAK-33140`) installed and the parameter `FeatureCtrl=NrDataAwareMobility` set to `ACTIVATED`.
* **Activity-Aware Target Scoring**: Requires Xn Resource Status Reporting toward neighbor gNodeBs (Xn Configuration (NR)). Without this reporting, scoring degrades gracefully to measurement-only ranking.
* **User- and Service-Specific Mobility**: Interworks with User- and Service-Specific Mobility. Per-service mobility profiles take precedence, and data-aware deferral applies within the profile's allowed bounds.
* **NR Traffic Offload**: Interworks with NR Traffic Offload. Offload-triggered handovers are classified as capacity moves and are always deferrable.

### Hardware Dependencies

* None. This is a baseband software-only feature.

### Network Dependencies

* **Xn Connectivity**: Xn connectivity to neighbor gNodeBs is required for load-based target scoring (optional but recommended).
* **5GC**: No 5G Core (5GC) dependencies.
* **UE/Core Network**: There are no core network or UE prerequisites.

### Limitations

* **Intra-NR Only**: Deferral applies only to intra-NR handovers. Inter-RAT events (e.g., EPS fallback) are never deferred.
* **URLLC and Voice Flows**: URLLC 5QIs (82–85) and 5QI 1 (voice) flows force the UE into the `LOW-activity` class handling (i.e., immediate execution) to protect their latency and continuity budgets.
* **Maximum Deferral Bound**: The maximum total deferral (offset + TTT extension + gap search) is bounded at 2 seconds.
* **High-Speed Mobility**: In very fast-moving UEs (estimated speed > 120 km/h), deferral is disabled automatically.
* **Load Report Aging**: Target scoring uses load reports up to 5 seconds old. In highly dynamic load conditions, scoring accuracy declines.

# Cross-References

* [Feature Overview](feature-overview.md)
* [Feature Operation](feature-operation.md)
* [Parameters](parameters.md)
* [Activation Procedure](activation-procedure.md)
