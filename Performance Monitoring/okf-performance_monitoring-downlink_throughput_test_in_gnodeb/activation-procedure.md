---
type: procedure
resource: data/vodafone-mvp/raw/Downlink Throughput Test in gNodeB.pdf#activation-procedure
title: Activation Procedure
description: Step-by-step procedure to activate, configure, and verify the Downlink
  Throughput Test feature in gNodeB.
tags:
- activation
- gnodeb
- downlink-throughput
- rancli
- testing
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T10:45:51+00:00'
  source_sha256: 3c1287a40a2f97f4
sources:
- resource: data/vodafone-mvp/raw/Downlink Throughput Test in gNodeB.pdf
  title: Downlink Throughput Test in gNodeB
---

This document describes the step-by-step activation procedure for the Downlink Throughput Test feature in the gNodeB. It outlines the traffic impact, preconditions, recommended rollout strategy, and the CLI commands required to activate, configure, and run an initial verification test.

## Traffic Impact and Preconditions

### Traffic Impact
* **At Activation:** None. Activating the feature only enables the test controller.
* **During Testing:** Air-interface impact occurs only when a test is actually started. 
  * In the default `RESOURCE_CAPPED` mode, the impact is bounded by the `maxTestPrbShare` parameter.
  * `FULL_BUFFER` testing, if used, requires a maintenance window due to its high resource utilization.

### Preconditions
* License key `FAK-30125` must be installed.
* If tests will target UEs by trace reference, the **NR UE Trace** feature must also be activated (see [Feature Dependencies](feature-depedencies.md)).

### Recommended Rollout Strategy
1. Activate the feature node-wide with `testModeAllowed=RESOURCE_CAPPED`.
2. Run verification tests on a few cells.
3. Only raise the policy to `FULL_BUFFER` on nodes where peak-rate acceptance testing is planned.

---

## Step-by-Step Activation and Verification

The activation and verification process consists of five steps using the `rancli` command-line tool.

### Step 1: Verify the License Key
Verify that the required license key is installed and enabled on the node.

```bash
rancli get NodeRoot=1,NrFunction=1,FeatureCtrl=DlThpTest licenseState
```
* **Expected Output:** `licenseState=ENABLED` (associated with key `FAK-30125`)

### Step 2: Activate the Feature
Activate the downlink throughput test feature controller.

```bash
rancli set NodeRoot=1,NrFunction=1,FeatureCtrl=DlThpTest featureState=ACTIVATED
```

### Step 3: Configure Node Test Policy
Set the allowed test mode and limit the maximum physical resource block (PRB) share for the test to prevent excessive air-interface impact.

```bash
rancli set NodeRoot=1,NrFunction=1,DlThpTest=1 testModeAllowed=RESOURCE_CAPPED maxTestPrbShare=20
```

### Step 4: Run a Verification Test
Execute a test against a connected User Equipment (UE) using its Cell Radio Network Temporary Identifier (C-RNTI) in the target cell.

```bash
rancli action NodeRoot=1,NrFunction=1,DlThpTest=1 startTest cell=N1A ueRef=0x4A21 duration=60 mode=RESOURCE_CAPPED
```
* *Note: This example targets UE `0x4A21` in cell `N1A` for a duration of 60 seconds in `RESOURCE_CAPPED` mode.*

### Step 5: Retrieve the Test Result
Read back the verdict and results of the executed test.

```bash
rancli get NodeRoot=1,NrFunction=1,DlThpTest=1 lastTestResult
```

### Post-Test Confirmation
After completing the first verification tests, confirm the following:
1. The performance counter `ctrTestsCompleted` increments.
2. The corresponding test record appears in the PM event file.

For more details on performance counters and event files, see [Performance Management](performance-management.md).

---

## Cross-References
* [Feature Overview](feature-overview.md)
* [Feature Dependencies](feature-depedencies.md)
* [Feature Operation](feature-operation.md)
* [Parameters](parameters.md)
* [Performance Management](performance-management.md)
* [Deactivation Procedure](deactivation-procedure.md)
