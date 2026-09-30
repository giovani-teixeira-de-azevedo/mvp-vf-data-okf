---
type: procedure
resource: data/vodafone-mvp/raw/Flexible PDCCH Monitoring.pdf#deactivation-procedure
title: Deactivation Procedure
description: Step-by-step procedure and RAN CLI commands to deactivate Flexible PDCCH
  Monitoring and recall UEs to dense monitoring.
tags:
- deactivation
- ran-cli
- pdcch-monitoring
- flexible-pdcch-monitoring
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-30T14:20:25+00:00'
  source_sha256: aa9b2ae7efac70ee
sources:
- title: Flexible PDCCH Monitoring
  resource: data/vodafone-mvp/raw/Flexible PDCCH Monitoring.pdf
---

This section details the procedure and CLI commands for deactivating the Flexible PDCCH Monitoring feature and returning connected User Equipments (UEs) to standard static monitoring.

## Deactivation Impact and Behavior

* **Traffic Impact:** None.
* **Switching and Configuration:**
  * No new switch or skip indications are issued.
  * UEs currently in the sparse group are switched back to dense monitoring immediately via DCI.
  * The Search Space Set Group (SSSG) configuration is removed from UEs at their next RRC reconfiguration.
  * From that point onward, all UEs behave as legacy static-monitoring UEs.

## Step-by-Step Procedure

1. **Disable per cell** (recalls all UEs to dense monitoring):
   ```bash
   rancli set NodeRoot=1,NrFunction=1,NrCell=N1A pdcchMonitoringMode=DISABLED
   ```

2. **Deactivate the feature control**:
   ```bash
   rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=FlexPdcchMonitoring featureState=DEACTIVATED
   ```

3. **Verify configuration and execution** (verify that sparse time stops accumulating in the following Result Output Period [ROP]):
   ```bash
   rancli get NodeRoot=1,NrFunction=1,NrCell=N1A pdcchMonitoringMode
   ```

# Cross-References

* [Activation Procedure](activation-procedure.md)
* [Parameters](parameters.md)
