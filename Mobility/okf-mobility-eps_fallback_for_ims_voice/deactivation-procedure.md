---
type: procedure
resource: data/vodafone-mvp/raw/EPS Fallback for IMS Voice.pdf#deactivation-procedure
title: DEACTIVATION PROCEDURE
description: Step-by-step procedure to deactivate EPS Fallback for IMS Voice and verify
  the configuration.
tags:
- EPS Fallback
- IMS Voice
- Deactivation
- RAN CLI
- VoNR
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T10:15:14+00:00'
  source_sha256: 75f948a2114073cd
sources:
- resource: data/vodafone-mvp/raw/EPS Fallback for IMS Voice.pdf
  title: EPS Fallback for IMS Voice
---

This section describes the step-by-step procedure to deactivate the EPS Fallback for IMS Voice feature, including traffic impact considerations and verification steps.

## Traffic Impact and Considerations

Deactivating this feature is **service-affecting for voice**. Before proceeding, review the following considerations:

* **VoNR Availability:** After deactivation, 5QI 1 flow requests on the affected cells are accepted for Voice over NR (VoNR) if Basic Voice over NR is active and permitted. Otherwise, voice establishment on NR Standalone (SA) will fail.
* **Deactivation Criteria:** Deactivate only when VoNR is verified on the cells, or as part of moving cells out of SA voice service. In the latter case, schedule a maintenance window and coordinate with the IMS/core teams.
* **Ongoing Calls:** Ongoing calls already on LTE are unaffected by this deactivation.
* **License Retention:** If deactivating because VoNR is now the primary voice service, keep the license installed so that the fallback path can be re-armed quickly if needed.

---

## Deactivation Steps

The deactivation process consists of three steps: disabling the per-cell policy, deactivating the feature control, and verifying the configuration.

### Step 1: Disable Per-Cell Policy
Disable the voice fallback mode on the target cell (e.g., `N1A`).

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A voiceFallbackMode=DISABLED
```

### Step 2: Deactivate the Feature Control
Deactivate the feature state for the EPS Fallback for IMS Voice feature.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=EpsFallbackImsVoice featureState=DEACTIVATED
```

### Step 3: Verify Configuration
Verify that the voice fallback mode has been successfully disabled on the cell.

```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A voiceFallbackMode
```

# Cross-References

* [Activation Procedure](activation-procedure.md)
* [Parameters](parameters.md)
