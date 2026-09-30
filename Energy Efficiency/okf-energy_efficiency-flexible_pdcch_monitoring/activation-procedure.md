---
type: procedure
resource: data/vodafone-mvp/raw/Flexible PDCCH Monitoring.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Details the step-by-step activation procedure, preconditions, traffic
  impact, and CLI commands for Flexible PDCCH Monitoring.
tags:
- flexible-pdcch-monitoring
- activation
- rancli
- sssg
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-30T15:03:52+00:00'
  source_sha256: 7c5ef5a708e47fda
sources:
- resource: data/vodafone-mvp/raw/Flexible PDCCH Monitoring.pdf
  title: Flexible PDCCH Monitoring
---

This section details the step-by-step activation procedure for the Flexible PDCCH Monitoring feature, including traffic impact, preconditions, recommended rollout strategy, and command-line execution steps.

## Operational Considerations

- **Traffic Impact:** None. The feature reconfigures capable UEs through standard RRC procedures as they connect or are reconfigured; existing connections adopt the configuration at their next reconfiguration. Monitoring adaptation itself is layer-1 signaled and reversible within slots. The procedure can be executed during business hours.
- **Preconditions:**
  - License key `FAK-31545` installed.
  - Connected Mode DRX active with an inactivity timer longer than the planned `sssgSwitchTimer`.
- **Recommended Rollout:** Start with `pdcchMonitoringMode=SSSG_ONLY` on a pilot cluster (switching only, no skipping), verify Switch Rate and latency KPIs for one week, then enable `SSSG_AND_SKIP`.
- **Post-Change Verification:** Confirm the Sparse Monitoring Share rises as UEs reconnect over the following hours, and check `ctrLateFirstPacket` stays proportionate.

## Procedure Steps

1. **Verify the license key is installed**
   ```bash
   rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=FlexPdcchMonitoring licenseState
   # Expected: licenseState=ENABLED (key FAK-31545)
   ```

2. **Activate the feature**
   ```bash
   rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=FlexPdcchMonitoring featureState=ACTIVATED
   ```

3. **Pilot: group switching only**
   ```bash
   rancli set NodeRoot=1,NrFunction=1,NrCell=N1A pdcchMonitoringMode=SSSG_ONLY
   rancli set NodeRoot=1,NrFunction=1,NrCell=N1A sparsePeriod=4 sssgSwitchTimer=8
   ```

4. **Enable skipping after pilot review**
   ```bash
   rancli set NodeRoot=1,NrFunction=1,NrCell=N1A pdcchMonitoringMode=SSSG_AND_SKIP maxSkipDuration=20
   ```

5. **Verify**
   ```bash
   rancli get NodeRoot=1,NrFunction=1,NrCell=N1A pdcchMonitoringMode
   ```

6. **test112342345354536456 step**
   Execute test112342345354536456 step.

# Cross-References

- [Parameters](parameters.md)
- [Performance Management](performance-management.md)
- [Deactivation Procedure](deactivation-procedure.md)
