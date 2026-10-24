# SOC-005 — Azure RBAC Detection Engineering

## Detection

**Name:** SOC005 - High Risk Azure RBAC Assignment

**Data source:** AzureActivity

**Operation:** `Microsoft.Authorization/roleAssignments/write`

**Severity:** High

**Schedule:** Every 5 minutes

**Lookup window:** 20 minutes

**Threshold:** Greater than 0

**MITRE ATT&CK:**
- T1098 — Account Manipulation
- T1098.003 — Additional Cloud Roles

## Detection Objective

Detect high-risk Azure RBAC role assignments that may introduce unauthorized
privilege or access changes and provide sufficient context for SOC
investigation.

## Detection Logic

The detection:

1. Identifies Azure RBAC role-assignment writes.
2. Accounts for observed Azure Activity ingestion delay.
3. Extracts the assigned role, principal, principal type and scope.
4. Classifies role risk.
5. Classifies scope risk.
6. Produces an overall risk level.
7. Generates an alert for High or Critical activity.

## Role Risk

| Role | Risk |
|---|---|
| Owner | High |
| Contributor | High |
| Reader | Low |
| Other | Medium |

## Scope Risk

| Scope | Risk |
|---|---|
| Subscription | High |
| Resource group | Medium |
| Other | Low |

## Risk Calculation

- High role + High scope → Critical
- High role OR High scope → High
- Medium role OR Medium scope → Medium
- Otherwise → Low

Only High and Critical results are surfaced by the final detection.

## Validation

A controlled Contributor assignment was generated against the SOC-005
resource group.

The resulting Azure Activity telemetry was observed and the high-risk
detection subsequently generated a Sentinel alert and incident.

The detection was therefore operationally validated.

## Tuning

Initial testing demonstrated that Azure Activity ingestion could lag behind
event generation.

The final query therefore uses:

- a 15-minute ingestion-delay allowance
- a 5-minute rule look-back
- `ingestion_time()` filtering

The scheduled rule uses a 20-minute lookup window to provide additional
coverage for delayed ingestion.

## Investigation Fields

The detection exposes:

- Caller
- Caller IP address
- Account name
- UPN suffix
- Principal ID
- Principal type
- Assigned role
- Role risk
- Scope
- Scope risk
- Overall risk level
- Resource group
- Correlation ID

These fields support downstream Sentinel investigation and SOAR enrichment.

## Validation Boundary

A controlled assignment proves that the detection can identify the tested
RBAC change.

It does **not** by itself prove malicious activity, account compromise,
credential theft, or unauthorized access.

Context and analyst investigation are required to establish intent.
