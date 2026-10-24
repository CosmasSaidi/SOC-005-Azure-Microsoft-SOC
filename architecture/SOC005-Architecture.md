# SOC-005 — Azure Microsoft SOC Architecture

## Overview

SOC-005 implements a Sentinel-centered Azure security operations architecture for collecting, detecting, investigating, hunting, and responding to Azure security telemetry.

## Architecture Flow

Azure Activity / Entra Audit Logs
        ↓
Log Analytics — law-soc005
        ↓
Microsoft Sentinel
        ↓
Analytics Rules / Threat Hunting
        ↓
Alert
        ↓
Incident
        ↓
Sentinel Automation Rule
        ↓
Logic App — SOC005-RBAC-Incident-Enrichment
        ↓
Automated Incident Comment

## Core Components

- Azure Activity telemetry provides Azure control-plane activity used for RBAC monitoring and investigation.
- Entra Audit Logs provide identity and directory-change telemetry within the available Entra ID licensing boundary.
- Log Analytics workspace law-soc005 provides the telemetry store.
- Microsoft Sentinel provides SIEM detection, investigation, hunting, incident management, and automation.
- Sentinel analytics rules identify high-risk Azure RBAC assignments.
- Sentinel automation rules invoke the incident-triggered Logic App playbook.
- The Logic App records automated enrichment in the Sentinel incident without performing automatic containment.

## Detection Flow

The primary SOC-005 detection monitors Microsoft.Authorization/roleAssignments/write activity in AzureActivity. The detection extracts the assigned role, principal, principal type, and scope, then applies role-risk and scope-risk classification before surfacing High or Critical activity.

## Response Flow

When a qualifying detection creates a Sentinel incident, the Sentinel automation rule invokes SOC005-RBAC-Incident-Enrichment. The playbook adds an automated enrichment comment to the incident. No account modification, containment, or destructive action is performed automatically.

## Architecture Boundary

SOC-005 is a Sentinel-centered Azure SOC implementation. Endpoint, email, and identity-provider workload telemetry associated with Microsoft Defender XDR was not available within the lab licensing environment and is therefore not represented as implemented telemetry.

## Validation

The architecture was validated through controlled Azure RBAC activity, Sentinel alert and incident generation, and successful incident-triggered Logic App execution.
