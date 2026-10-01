---
type: concept
resource: data/vodafone-mvp/raw/EPS Fallback for IMS Voice.pdf#feature-depedencies
title: Feature Dependencies
description: Outlines the feature, hardware, network dependencies, and limitations
  for EPS Fallback for IMS Voice.
tags:
- EPS Fallback
- Dependencies
- Limitations
- Interworking
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T10:15:07+00:00'
  source_sha256: 57066a436237f771
sources:
- resource: data/vodafone-mvp/raw/EPS Fallback for IMS Voice.pdf
  title: EPS Fallback for IMS Voice
---

This section details the feature, hardware, network dependencies, and limitations associated with the EPS Fallback for IMS Voice feature. Because fallback spans two Radio Access Technologies (RATs) and the 5GC/EPC interworking architecture, successful deployment requires careful coordination across the NR node, the underlying LTE layer, and the core network.

> [!IMPORTANT]
> Misconfigured LTE target data is the dominant field cause of fallback failures. It is critical to validate the target carrier list on every cell.

---

## Feature Dependencies

* **NR Standalone:** Requires NR Standalone (SA) activated on the node.
* **Licensing and Control:** Requires a valid license key (`FAK-33121`) installed and the parameter `FeatureCtrl=EpsFallbackImsVoice` set to `ACTIVATED`.
* **Neighbor Relations:** The handover-based method requires inter-RAT handover neighbor relations toward E-UTRAN. NR Automated Neighbor Relations (ANR) is strongly recommended to keep these relations current.
* **NR Emergency Fallback to LTE:** Interworks with NR Emergency Fallback to LTE. Emergency calls follow that feature's dedicated logic and are not affected by this feature's per-cell method selection.
* **Basic Voice over NR (VoNR):** Interworks with Basic Voice over NR. On cells where VoNR is enabled, fallback acts as the secondary path for UEs or coverage situations where VoNR is not permitted.

## Hardware Dependencies

* **None:** The feature is baseband software only and runs on all supported baseband units.

## Network Dependencies

* **Core Interworking:** 5GC and EPC interworking must be in place. Handover-based fallback requires the N26 interface between the Access and Mobility Management Function (AMF) and the Mobility Management Entity (MME).
* **LTE Layer Support:** The underlying LTE layer must support VoLTE (QCI 1) with adequate capacity and coverage overlapping the NR SA footprint.
* **IMS Configuration:** The IP Multimedia Subsystem (IMS) must be configured to serve subscribers via both 5GC and EPC access.
* **UE Capabilities:** User Equipments (UEs) must support E-UTRAN and, for the handover method, inter-RAT handover from NR (per TS 38.306 capabilities).

## Limitations

* **Redirect-based Fallback:** Redirect-based fallback without measurements can send UEs to an LTE carrier that is weak at the UE's location. It is recommended to enable measurement-based target selection wherever multiple LTE layers exist.
* **Video Flows:** Video (5QI 2) flows accompanying the voice call are also moved to EPS. Any non-GBR flows follow via the normal PDU session transfer.
* **RRC State:** Fallback is not performed for UEs in `RRC_INACTIVE` until they resume.
* **Target Frequencies:** A maximum of 8 LTE target frequencies can be configured per NR cell for B1 measurement.

---

# Cross-References

* [Feature Overview](feature-overview.md) — For an overview of the EPS Fallback feature.
* [Feature Operation](feature-operation.md) — For details on how the fallback methods operate.
* [Parameters](parameters.md) — For configuration parameters including `FeatureCtrl`.
* [Activation Procedure](activation-procedure.md) — For step-by-step activation instructions.
