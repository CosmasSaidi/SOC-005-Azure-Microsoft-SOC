# SOC-005 — Detection Coverage Matrix

## Purpose

This matrix records the detection and security-operation coverage implemented and validated in SOC-005. It distinguishes fully validated detections from telemetry-validated or experimental scenarios and prevents unsupported claims in the final portfolio documentation.

| Use Case | Data Source | Detection / Capability | Validation | ATT&CK | Status |
|---|---|---|---|---|---|
| Azure RBAC assignment | AzureActivity | Azure RBAC role assignment detection | Controlled RBAC assignment validated | T1098.003 | Validated |
| High-risk Azure RBAC assignment | AzureActivity | Role + scope risk classification | Controlled Contributor assignment validated | T1098.003 | Validated / Tuned |
| Entra user modification | AuditLogs | Entra audit telemetry / candidate detection | Controlled user creation validated | Account Manipulation | Telemetry validated |
| Azure Activity ingestion delay | AzureActivity | Ingestion-delay-aware query design | Ingestion timing measured and query tuned | N/A | Validated |
| Automated incident enrichment | SecurityIncident / Sentinel incident trigger | Logic App SOAR enrichment | End-to-end incident-triggered playbook validated | N/A | Validated |
| Threat hunting | AzureActivity / Sentinel data | RBAC privilege-change hunting | Hunting queries executed against collected telemetry | T1098.003 | Validated |
| Operational monitoring | Sentinel / SecurityIncident / SecurityAlert | Detection volume and timing metrics | Metrics queries executed against collected data | N/A | Validated |

## Detection Coverage Boundaries

### Fully validated

- Azure RBAC role-assignment activity detection.
- High-risk RBAC detection using role and scope risk.
- Controlled Contributor-role assignment detection.
- Ingestion-delay-aware scheduled detection logic.
- Sentinel incident creation from the high-risk RBAC analytic rule.
- Incident-triggered Logic App enrichment.
- RBAC threat hunting against collected Azure Activity telemetry.
- Operational measurement of incident and alert activity.

### Telemetry validated but not promoted to a fully validated detection

- Microsoft Entra Audit Logs user modification activity.

The controlled Entra test demonstrates that the relevant audit telemetry is generated and available for investigation. It does not by itself establish a production-ready detection rule.

## Detection Validation Boundary

A controlled assignment proves that the detection can identify the tested RBAC change.

It does not by itself prove:

- malicious activity;
- account compromise;
- credential theft;
- unauthorized access; or
- attacker intent.

Context and analyst investigation are required to establish intent.

## Licensing / Telemetry Boundary

SOC-005 is a Sentinel-centered Azure SOC implemented using the telemetry and licensing available in Azure for Students.

The project does not claim full Microsoft Defender XDR coverage.

The following workload telemetry was not available for this implementation:

- Microsoft Defender for Endpoint telemetry;
- Microsoft Defender for Office 365 telemetry;
- Microsoft Defender for Identity telemetry;
- Microsoft Entra sign-in logs requiring Entra ID P1/P2;
- broader Defender XDR incident correlation.

These limitations are intentional and documented rather than simulated or fabricated.

## Portfolio Positioning

SOC-005 extends the portfolio from endpoint, Windows/Active Directory, identity, and network security operations into cloud-native Microsoft security operations.

Its primary evidence areas are:

- Microsoft Sentinel;
- Azure Activity telemetry;
- Microsoft Entra Audit Logs;
- KQL detection engineering;
- cloud privilege/RBAC monitoring;
- detection tuning;
- threat hunting;
- incident investigation;
- Sentinel automation and Logic Apps;
- operational security metrics.

SOC-005 should therefore be presented as a cloud-native Microsoft SOC engineering project rather than as a claim of full Microsoft Defender XDR deployment.
