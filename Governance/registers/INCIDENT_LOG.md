# Incident Log

This log records every operational, security, or privacy incident affecting
TeachLink Mobile. Its purpose is to give the project one reference for what
happened, how it was handled, and what changed as a result.

## Log Columns

Each incident records the following columns:

| Column | Description |
|--------|-------------|
| Incident ID | Sequential identifier (for example, `INC-0001`). |
| Summary | Short description of the incident. |
| Category | Security, privacy, availability, release, or data. |
| Severity | Severity rating (see below). |
| Detected | Date and time the incident was detected. |
| Resolved | Date and time the incident was resolved. |
| Impact | Effect on users, contributors, or releases. |
| Root cause | Why the incident happened. |
| Remediation | Action taken to resolve it. |
| Follow-up | Preventive action, owner, and target date. |
| Status | Open, Mitigated, or Closed. |

## Log

| Incident ID | Summary | Category | Severity | Detected | Status |
|-------------|---------|----------|----------|----------|--------|
| _none recorded_ | | | | | |

## Severity and Status

Severity follows
[`Governance/SECURITY_SEVERITY_RUBRIC.md`](../SECURITY_SEVERITY_RUBRIC.md).
Status values are:

- **Open** — detected and not yet contained.
- **Mitigated** — contained, with follow-up remaining.
- **Closed** — resolved and follow-up complete.

## Ownership and Review

- Maintained by the Security Officer, with the release owner for release incidents.
- Every incident gets a root cause within **5 business days**.
- The log is reviewed **monthly** and summarized in the governance report.
- Entries are append-only; closed incidents are never deleted.

## Related Documents

- `Governance/SECURITY_SEVERITY_RUBRIC.md`
- `Governance/registers/RISK_REGISTER.md`
