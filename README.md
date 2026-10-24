# SOC-005 — Azure Microsoft SOC

A hands-on Azure security operations lab built around Microsoft Sentinel, Azure Activity telemetry, Entra Audit Logs, KQL, detection engineering, investigation, threat hunting, and incident-triggered automation.

The main focus of this project was not simply getting Sentinel running. I wanted to build and validate a small cloud SOC workflow from telemetry collection through detection, investigation, tuning, hunting, and response automation.

---

## What I Built

SOC-005 monitors activity within an Azure environment and uses Microsoft Sentinel as the central SIEM.

The primary detection use case is **high-risk Azure RBAC role assignment activity**.

The detection monitors:

`Microsoft.Authorization/roleAssignments/write`

It extracts the assigned role, principal, principal type, and scope from Azure Activity data and then evaluates the risk of the change.

For example:

- Owner → High role risk
- Contributor → High role risk
- Reader → Low role risk
- Subscription scope → High scope risk
- Resource group scope → Medium scope risk

The resulting role and scope risk are combined to produce an overall risk level. High and Critical activity is surfaced for investigation.

---

## Environment

| Component | Implementation |
|---|---|
| Cloud | Microsoft Azure |
| SIEM | Microsoft Sentinel |
| Log Analytics | `law-soc005` |
| Resource Group | `rg-soc005` |
| Primary telemetry | Azure Activity |
| Identity telemetry | Entra Audit Logs |
| Detection language | KQL |
| Automation | Azure Logic Apps |
| Automation trigger | Sentinel incident |
| MITRE ATT&CK | T1098 / T1098.003 |

The environment was built using an Azure for Students subscription.

---

## Detection Engineering

The RBAC detection went through several stages rather than being treated as a single KQL query.

I first established the available Azure Activity telemetry and identified the RBAC role-assignment operation.

The detection was then built to:

1. Identify RBAC role-assignment writes.
2. Extract the relevant request properties.
3. Identify the assigned role.
4. Identify the affected principal.
5. Determine the assignment scope.
6. Classify role risk.
7. Classify scope risk.
8. Calculate an overall risk level.
9. Generate a Sentinel alert and incident for High or Critical activity.

### Ingestion-delay tuning

One of the useful findings from the lab was that the event timestamp and the time at which the event became available to the SIEM were not always the same.

The final scheduled detection therefore accounts for observed ingestion delay using an extended look-back and `ingestion_time()` filtering.

The rule runs every five minutes with a 20-minute lookup window.

That tuning was based on observed telemetry rather than choosing the values arbitrarily.

---

## Validation

I used controlled Azure RBAC changes to validate the detection.

A controlled Contributor assignment against the SOC-005 resource group produced the expected Azure Activity telemetry and subsequently generated a Sentinel alert and incident.

The validation also provided the data needed to investigate the event, including:

- Caller
- Caller IP address
- Principal ID
- Principal type
- Assigned role
- Scope
- Resource group
- Correlation ID
- Risk classification

The test was deliberately controlled. It demonstrates that the detection can identify the tested RBAC change; it is not presented as evidence of an actual compromise or malicious actor.

---

## Investigation

The generated incidents were investigated using the available Azure and Sentinel telemetry.

The investigation process focused on establishing:

- who performed the change
- which role was assigned
- which principal was affected
- where the role was assigned
- what scope was affected
- what IP address was associated with the activity
- whether the activity represented a high-risk privilege change
- whether the event was a successful change or an expected edge case

A duplicate role-assignment attempt was also observed during testing and classified separately from the successful privilege change.

This helped keep the detection focused on meaningful activity rather than treating every related event as an incident of equal importance.

---

## Threat Hunting

The same telemetry was used outside the analytic rule for proactive hunting.

The hunting queries examine Azure RBAC activity and apply the same role and scope context used by the detection.

This provides a way to look for privilege changes that may not necessarily have originated from an existing alert.

The hunting work is documented separately from the production analytic rule so that detection logic and exploratory analysis remain distinguishable.

---

## SOAR

SOC-005 includes an incident-triggered Logic App playbook:

`SOC005-RBAC-Incident-Enrichment`

The playbook is invoked through a Sentinel automation rule when the high-risk RBAC detection creates an incident.

Its current action is intentionally limited to incident enrichment. It adds an automated comment recording that the playbook executed.

No account modification, privilege removal, or automatic containment is performed.

The end-to-end workflow was validated through a controlled RBAC event and a successful Logic App execution.

---

## Operational Visibility

An operational Sentinel workbook was created to provide a compact view of the environment.

It currently covers:

- Azure Activity volume
- RBAC activity
- RBAC role and scope risk
- High-risk RBAC assignments

The workbook is intended as an analyst-facing operational view rather than a collection of decorative charts.

---

## Evidence

The project was documented as an evidence-driven lab.

Raw screenshots were preserved separately from selected portfolio evidence.

The evidence set includes material covering:

- Azure and Sentinel configuration
- telemetry
- detection creation
- controlled validation
- investigation
- tuning
- threat hunting
- SOAR execution
- operational measurements

SHA-256 hashes were generated for the raw screenshot set, and the selected evidence was independently verified against its recorded hashes.

This keeps the portfolio evidence tied to the actual work performed in the lab.

---

## MITRE ATT&CK

The primary detection mapping is:

**T1098 — Account Manipulation**

**T1098.003 — Additional Cloud Roles**

The mapping reflects the privilege-change behavior represented by Azure RBAC role assignments.

The ATT&CK mapping is used to provide detection context; it is not treated as proof that the controlled test represented adversary activity.

---

## Limitations

This project intentionally documents its environment boundaries.

The available licensing did not provide the Microsoft Defender XDR workload telemetry required to demonstrate endpoint, email, and other Defender workload detections. Those capabilities are therefore not claimed as implemented in this project.

The lab used Entra ID Free, so Entra Sign-in Logs were not available within the implemented telemetry scope. Entra Audit Logs were available and were used for directory-change telemetry validation.

As a result, SOC-005 should be viewed as a **Sentinel-centered Azure SOC implementation**, rather than a full Microsoft Defender XDR SOC.

---

## What This Project Demonstrates

This project is primarily evidence of practical work in:

- Microsoft Sentinel
- Azure security monitoring
- KQL
- Detection engineering
- Detection tuning
- Azure RBAC monitoring
- Cloud privilege-change investigation
- Threat hunting
- Sentinel incident management
- Logic Apps / SOAR
- MITRE ATT&CK mapping
- Security telemetry analysis
- Evidence handling and documentation

It also builds on the Windows, Linux, identity, and network security work in the rest of my SOC portfolio by adding a cloud-native Microsoft security environment.

---

## Repository Structure

```text
SOC-005-Azure-Microsoft-SOC/
├── architecture/
├── detections/
├── docs/
├── evidence/
├── investigations/
├── kql/
├── hunting/
├── metrics/
├── response/
├── automation/
└── workbooks/
```

The repository separates detection logic, investigation material, operational documentation, automation, and evidence rather than keeping the entire project in a single report.

---
