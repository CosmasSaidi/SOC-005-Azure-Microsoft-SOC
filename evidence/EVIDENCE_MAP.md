# SOC-005 Evidence Map

## Project
SOC-005 — Microsoft SOC / Azure Sentinel-Centered Detection & Response

## Evidence Scope

This evidence set documents the operational validation of the SOC-005 Azure environment.

### 01 — Azure / Sentinel Environment
- Azure resource group and deployed resources
- Log Analytics workspace
- Microsoft Sentinel onboarding
- Sentinel configuration and health

### 02 — Telemetry
- Azure Activity telemetry
- Administrative activity
- RBAC role-assignment events
- Sentinel log queries and validation

### 03 — Detection Engineering
- Azure RBAC role-assignment detection
- High-risk RBAC detection
- Role-risk classification
- Scope-risk classification
- Detection rule configuration
- MITRE ATT&CK mapping

### 04 — Detection Validation
- Controlled Reader assignment
- Controlled Contributor assignment
- Raw telemetry validation
- Detection alert generation
- High-risk alert generation
- Incident creation

### 05 — Investigation
- Sentinel incidents
- Alert entities and enrichment
- Caller identity
- Source IP
- Principal identity
- Assigned role
- Scope
- Correlation identifiers
- Benign/expected classification where applicable

### 06 — Detection Tuning
- Azure Activity ingestion-delay investigation
- Ingestion-time analysis
- Detection look-back adjustment
- Final high-risk detection logic

### 07 — Threat Hunting
- Sentinel Advanced Hunting / KQL investigation
- RBAC activity investigation
- Supporting telemetry queries

### 08 — SOAR
- Sentinel automation rule
- Logic App playbook
- Sentinel incident trigger
- Automated incident comment
- Successful Logic App execution
- Playbook modification recorded in incident activity

### 09 — Operational Validation
- Detection execution health
- Alert/incident metrics
- Detection-to-incident timing measurements
- Final high-risk detection validation

## Evidence Handling

The `raw/` directory contains the recovered screenshot evidence set.

The screenshots are retained without modification.

SHA-256 hashes are stored in:
`evidence/hashes/screenshots.sha256`

The `_review/` directory is a working review area and is not the authoritative evidence package.
