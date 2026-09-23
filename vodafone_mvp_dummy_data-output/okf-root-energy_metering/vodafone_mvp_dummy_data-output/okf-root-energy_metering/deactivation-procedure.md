---
type: concept
resource: data/vodafone-mvp/raw/Energy Metering.pdf#deactivation-procedure
title: DEACTIVATION PROCEDURE
description: Outlines the operational considerations, impact, and CLI commands for
  deactivating the Energy Metering feature.
tags:
- energy-metering
- deactivation
- rancli
- vodafone
status: draft
generated:
  by: enricher_agent/gemini-3.6-flash
  at: '2026-09-23T10:26:28+00:00'
  source_sha256: 7eb0b78ed5474cc4
sources:
- title: Energy Metering
  resource: data/vodafone-mvp/raw/Energy Metering.pdf
---

This section describes the procedure for deactivating the Energy Metering feature, including traffic impact, pre-deactivation considerations, and execution commands.

### Operational Considerations and Impact

- **Traffic Impact:** None. Deactivation stops energy accumulation and counter reporting; no radio behavior changes.
- **Attribute Retention:** Lifetime cumulative energy attributes retain their last values.
- **Pre-deactivation Verification:** Before deactivating, confirm that no active reporting pipeline (such as a sustainability dashboard or savings verification for an energy feature trial) depends on the node's counters, as gaps in energy time series are awkward to interpolate.
- **Configuration Retention:** Step 1 deactivates the feature control; step 2 verifies that momentary attributes report as unavailable while configuration is retained for re-activation.

### Execution Steps

1. **Deactivate the feature:**
   ```bash
   rancli set NodeRoot=1,EquipmentFunction=1,FeatureCtrl=EnergyMetering featureState=DEACTIVATED
   ```

2. **Verify deactivation:**
   ```bash
   rancli get NodeRoot=1,EquipmentFunction=1,FeatureCtrl=EnergyMetering featureState
   rancli get NodeRoot=1,EquipmentFunction=1,RadioUnit=RU1 momentaryPower
   ```

# Cross-References

- [Activation Procedure](activation-procedure.md)
