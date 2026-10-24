# SOC-005 — Limitations and Lessons Learned

## Limitations

### Microsoft Defender XDR Workloads

SOC-005 is a Sentinel-centered Azure SOC implementation. Microsoft Defender
XDR workload telemetry was not available within the lab licensing
environment and is therefore not represented as implemented telemetry.

This project does not claim endpoint, email, or identity-provider workload
coverage through Microsoft Defender XDR.

### Entra ID Sign-in Telemetry

The lab used Entra ID Free. Sign-in Logs were therefore not available for
the implemented Sentinel telemetry scope.

Entra Audit Logs were available and were used for directory-change
telemetry validation.

### Detection Validation Boundary

The RBAC detection was validated using controlled role-assignment activity.

This demonstrates that the detection can identify the tested Azure RBAC
change. It does not independently prove malicious activity, account
compromise, credential theft, unauthorized access, or attacker intent.

### Telemetry Timing

Testing demonstrated that Azure Activity ingestion can lag behind the
original activity timestamp.

The detection therefore incorporates an ingestion-delay allowance,
`ingestion_time()` filtering, and an extended scheduled-rule lookup window.

## Lessons Learned

### Ingestion Delay Must Be Designed For

Detection logic cannot assume that cloud activity is immediately available
in the SIEM. Detection scheduling, look-back windows, and ingestion timing
must be considered together.

### Risk Classification Improves Detection Context

Detecting every RBAC assignment equally would produce less useful analyst
context. Combining role risk with scope risk provides a more meaningful
priority signal for investigation.

### Validation Must Be Controlled

A controlled change provides a reproducible way to verify telemetry,
detection logic, alert generation, incident creation, and downstream
automation without claiming that the test itself represents malicious
activity.

### SOAR Should Be Deliberate

The implemented playbook performs incident enrichment rather than automatic
containment. This keeps the response action appropriate for a lab-validated
detection while demonstrating incident-triggered automation.

### Evidence Quality Matters

Raw screenshots were preserved separately from selected portfolio evidence,
with SHA-256 hashes used to verify evidence integrity.

### Scope Boundaries Should Be Explicit

A credible SOC project should clearly distinguish implemented telemetry,
validated detections, telemetry-only validation, and capabilities excluded
because of licensing or environmental constraints.
