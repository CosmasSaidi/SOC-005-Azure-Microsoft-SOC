# SOC-005 Evidence Register

## Project
SOC-005 — Microsoft SOC / Azure Sentinel Detection & Incident Response

## Environment
- Azure for Students
- Resource Group: rg-soc005
- Log Analytics Workspace: law-soc005
- Microsoft Sentinel: enabled on law-soc005
- Region: Italy North
- Identity: Microsoft Entra ID Free

## Telemetry Sources
- Azure Activity
- Microsoft Entra Audit Logs

## Detection Engineering
- Azure RBAC role-assignment detection
- High-risk Azure RBAC assignment detection
- KQL-based risk classification
- Ingestion-delay-aware detection logic

## Investigation
- Azure RBAC role-assignment investigation
- Contributor role assignment investigation
- Caller/IP/context analysis
- Benign duplicate role-assignment edge case
- Detection-to-incident timing analysis

## SOAR
- Logic App: SOC005-RBAC-Incident-Enrichment
- Sentinel incident trigger
- Automated incident comment
- Sentinel automation rule
- Successful end-to-end playbook execution

## Validation
- Positive RBAC telemetry validated
- High-risk Contributor detection validated
- Ingestion delay identified and accounted for
- SOAR execution validated
- Cleanup completed

## Known Scope Limitations
- No Microsoft Defender XDR workload telemetry
- No Microsoft Defender for Endpoint telemetry
- No Defender for Office 365 telemetry
- No Defender for Identity telemetry
- Entra ID Free does not provide Sign-in Logs in this environment
- Detection thresholds were controlled-lab thresholds, not production-calibrated thresholds
- IP reputation/geolocation is contextual evidence, not proof of malicious activity

## Evidence Status
Architecture: pending packaging
Telemetry: pending packaging
Detections: pending packaging
Investigations: pending packaging
Automation/SOAR: pending packaging
Metrics: pending packaging
QA: pending packaging
