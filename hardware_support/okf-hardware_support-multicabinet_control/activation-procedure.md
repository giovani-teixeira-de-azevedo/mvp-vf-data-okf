---
type: procedure
resource: data/vodafone-mvp/raw/Multicabinet Control.pdf#activation-procedure
title: ACTIVATION PROCEDURE
description: Step-by-step CLI procedure to verify licensing, activate Multicabinet
  Control, discover secondary SCUs, and bind cabinets.
tags:
- multicabinet-control
- activation
- rancli
- configuration
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-09-14T16:45:41+00:00'
  source_sha256: d7d7501128c9f91f
sources:
- title: Multicabinet Control
  resource: data/vodafone-mvp/raw/Multicabinet Control.pdf
---

The Multicabinet Control activation procedure provides step-by-step instructions for enabling multicabinet control functionality on the NodeRoot, discovering secondary Support Control Units (SCUs) via the site control bus, and binding and configuring the expansion cabinets.

## Traffic Impact & Considerations

*   **Traffic Impact:** None. Discovery, binding, and configuration synchronization touch only support hardware; radio and transport functions are unaffected.
*   **Transient Effects:** While setpoints synchronize, a secondary cabinet's climate system may briefly change fan speed.
*   **Scheduling:** Activation can be run safely during business hours.

## Preconditions

Before proceeding with the activation, ensure the following prerequisites are met:
*   License key **FAK-51060** is installed on the node (see [Feature Dependencies](feature-depedencies.md) for licensing requirements).
*   SCU hardware generation is verified in every cabinet.
*   Control-bus cabling between cabinets is installed and tested.
*   SCU serial numbers are recorded from the site survey for explicit binding.

### Recommended Execution Strategy
Activate, discover, and bind cabinets one at a time, verifying each before proceeding to the next. Review the site-wide load-shed priorities as a final deliberate step.

---

## Step-by-Step Procedure

### Step 1: Verify the License Key is Installed
Run the command below to ensure the correct license is enabled.

```bash
rancli get NodeRoot=1,EquipmentFunction=1,FeatureCtrl=MulticabinetControl licenseState
```

**Expected Output:**
```text
licenseState=ENABLED (key FAK-51060)
```

### Step 2: Activate the Feature
Set the feature state to active.

```bash
rancli set NodeRoot=1,EquipmentFunction=1,FeatureCtrl=MulticabinetControl featureState=ACTIVATED
```

### Step 3: Discover Secondary SCUs on the Site Control Bus
Scan the control bus to identify the connected secondary support control units.

```bash
rancli action NodeRoot=1,EquipmentFunction=1 scanSupportBus
```

**Expected Return:**
The command returns the discovered SCU serial numbers, for example:
```text
SCU-A1B2C3, SCU-D4E5F6
```

### Step 4: Create and Bind an Expansion Cabinet
Create the cabinet managed object (MO), set its role to secondary, and bind it to the discovered SCU serial number.

```bash
rancli set NodeRoot=1,EquipmentFunction=1,Cabinet=2 cabinetRole=SECONDARY boundScuSerial=SCU-A1B2C3
```

### Step 5: Configure Climate and Shed Priority
Define the climate setpoint and load-shed priority for the bound expansion cabinet (see [Parameters](parameters.md)).

```bash
rancli set NodeRoot=1,EquipmentFunction=1,Cabinet=2 climateSetpoint=25 loadShedPriority=2
```

### Step 6: Verify Supervision
Verify that the secondary cabinet is operational and its telemetry is being correctly supervised.

```bash
rancli get NodeRoot=1,EquipmentFunction=1,Cabinet=2 operationalState currentTemperature
```

### Step 7: Repeat and Validate (Per Cabinet)
*   Repeat **Steps 4 to 6** for each remaining expansion cabinet.
*   Once all cabinets are bound and configured, trigger a manual battery test on one battery cabinet to confirm end-to-end control.

---

## Cross-References

*   [Feature Dependencies](feature-depedencies.md) — License key and hardware prerequisites.
*   [Parameters](parameters.md) — Description of MO parameters including `cabinetRole`, `boundScuSerial`, `climateSetpoint`, and `loadShedPriority`.
*   [Deactivation Procedure](deactivation-procedure.md) — Instructions on how to disable the feature or tear down cabinet configurations.
