---
type: concept
resource: data/vodafone-mvp/raw/NR Automated Neighbor Relations.pdf#feature-operation
title: Feature Operation
description: Detailed operational flow of the NR Automated Neighbor Relations (ANR)
  feature, including neighbor detection, CGI resolution, and relation maintenance.
tags:
- ANR
- Neighbor Detection
- CGI Resolution
- Relation Maintenance
- Xn Setup
status: draft
generated:
  by: enricher_agent/gemini-3.5-flash
  at: '2026-10-01T16:53:45+00:00'
  source_sha256: 779a3656e2c9f14c
sources:
- resource: data/vodafone-mvp/raw/NR Automated Neighbor Relations.pdf
  title: NR Automated Neighbor Relations
---

The Automated Neighbor Relations (ANR) feature runs continuously on a per-cell basis to detect and maintain neighbor relations. It automates the discovery of neighbor cells and manages the lifecycle of neighbor relations to optimize mobility and handovers.

## Neighbor Detection and CGI Resolution

ANR leverages the standard mobility measurement configuration to detect potential neighbor cells:

1. **Measurement Reports**: Any A3, A4, or A5 measurement report (or B1/B2 report for inter-RAT) that carries an unknown Physical Cell Identity (PCI) on a monitored Absolute Radio Frequency Channel Number (ARFCN) triggers the process by queuing a Cell Global Identifier (CGI) resolution task.
2. **CGI Resolution Request**: The scheduler issues a `reportCGI` command to a suitable User Equipment (UE).
   - **UE Selection**: The scheduler prefers UEs in good radio conditions with low activity to minimize the performance impact of autonomous gaps.
   - **Timer Guard**: The CGI resolution process is guarded by the `cgiReportTimer` parameter.

## Relation Creation and Xn Setup

Upon successful receipt of the CGI:

- **Relation Creation**: A new neighbor relation is created. This relation inherits default mobility attributes from the corresponding frequency relation.
- **Xn Interface Setup**: If the `autoXnSetup` parameter is set to `true`, the gNodeB uses the NG Configuration Transfer procedure to resolve the peer node and initiate Xn interface setup.

## Maintenance Loop

The ANR maintenance loop runs periodically to evaluate and clean up neighbor relations:

- **Evaluation Window**: Each neighbor relation is evaluated once per 24-hour window.
- **Relation Deletion**: Relations with zero handover attempts for a duration specified by `relationRemovalTime` (in days) are automatically deleted, unless the relation is protected by the `noRemove` attribute.
- **Failure Monitoring**: If a relation's handover failure ratio exceeds 50% over 100 or more attempts, the system raises an alarm for engineering review. The relation is not automatically blocked, ensuring that the operator retains control over blacklisting decisions.

# Cross-References

- [Parameters](parameters.md) — Configuration parameters governing ANR operation, such as `cgiReportTimer`, `autoXnSetup`, `relationRemovalTime`, and `noRemove`.
- [Feature Overview](feature-overview.md) — High-level overview of the Automated Neighbor Relations feature.
