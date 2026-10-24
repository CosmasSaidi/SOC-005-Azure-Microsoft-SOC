# SOC-005 — Operational Workbook

## Purpose

The SOC-005 Operational Workbook provides an analyst-facing operational view of Azure Activity, Azure RBAC activity, high-risk privilege assignments, Sentinel incidents, and detection timing.

## Primary Objectives

- Monitor Azure Activity volume.
- Monitor Azure RBAC assignment activity.
- Identify high-risk role assignments.
- Visualize role and scope risk.
- Review high-risk incident activity.
- Measure detection-to-incident timing.
- Provide a compact operational view of SOC-005 telemetry.

## Data Sources

- AzureActivity
- SecurityAlert
- SecurityIncident

## Design Principle

The workbook uses the same KQL logic and telemetry already validated during SOC-005 detection engineering and investigation.

It is intended for operational monitoring and analyst investigation rather than executive reporting alone.

## Evidence Boundary

Workbook visualizations represent the telemetry available within the SOC-005 Azure for Students environment.

The workbook does not imply availability of Microsoft Defender XDR, Defender for Endpoint, Defender for Office 365, Defender for Identity, or Entra sign-in telemetry that was unavailable under the implemented licensing boundary.

## Portfolio Role

The workbook demonstrates the operationalization of Sentinel detections after detection engineering, validation, investigation, tuning, hunting, and SOAR implementation.
