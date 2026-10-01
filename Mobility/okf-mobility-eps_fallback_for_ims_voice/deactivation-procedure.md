---
type: procedure
resource: data/vodafone-mvp/raw/EPS Fallback for IMS Voice.pdf#deactivation-procedure
title: DEACTIVATION PROCEDURE
description: Step-by-step procedure to deactivate the EPS Fallback for IMS Voice feature
  and disable the per-cell policy.
tags:
- eps-fallback
- ims-voice
- deactivation
- rancli
- vonr
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T11:01:39+00:00'
  source_sha256: 75f948a2114073cd
sources:
- title: EPS Fallback for IMS Voice
  resource: data/vodafone-mvp/raw/EPS Fallback for IMS Voice.pdf
---

This section describes the step-by-step procedure to deactivate the EPS Fallback for IMS Voice feature and disable the per-cell policy. It includes traffic impact considerations, prerequisites, and the required CLI commands.

## Traffic Impact and Considerations

Deactivating this feature is **service-affecting for voice services**. Before proceeding, review the following operational impacts and guidelines:

*   **VoNR Dependency:** After deactivation, 5QI 1 flow requests on the affected cells are accepted for Voice over NR (VoNR) only if Basic Voice over NR is active and permitted. If VoNR is not active or permitted, voice establishment on NR Standalone (SA) will fail.
*   **Deactivation Criteria:** Only deactivate this feature when VoNR has been verified on the cells, or as part of decommissioning cells from SA voice service. If moving cells out of SA voice service, schedule a maintenance window and coordinate with the IMS and core network teams.
*   **Ongoing Calls:** Ongoing calls that have already fallen back to LTE are unaffected by this deactivation.
*   **License Retention:** If deactivating because VoNR is now the primary voice service, keep the feature license installed so that the EPS fallback path can be re-armed quickly if needed.

## Deactivation Steps

The deactivation process consists of three steps: disabling the per-cell policy, deactivating the feature control, and verifying the configuration.

### Step 1: Disable Per-Cell Policy
Disable the voice fallback mode on the target cell (e.g., `N1A`).

```bash
rancli set NodeRoot=1,NrFunction=1,NrCell=N1A voiceFallbackMode=DISABLED
```

### Step 2: Deactivate the Feature Control
Deactivate the global feature state for EPS Fallback for IMS Voice.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=EpsFallbackImsVoice featureState=DEACTIVATED
```

### Step 3: Verify Configuration
Verify that the voice fallback mode has been successfully disabled on the cell.

```bash
rancli get NodeRoot=1,NrFunction=1,NrCell=N1A voiceFallbackMode
```

# Cross-References

*   [Activation Procedure](activation-procedure.md) — For details on enabling the feature and configuring the cell policies.
*   [Parameters](parameters.md) — For details on `voiceFallbackMode` and other related configuration parameters.
