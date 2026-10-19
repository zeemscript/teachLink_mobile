# SLA Breach Log

This register records every breach of a response or review target so that the
project can find root causes and prevent repeats.

## Breach Record Fields

Each entry records the following fields:

| Field | Description |
|-------|-------------|
| Breach ID | Sequential identifier (for example, `SLA-0001`). |
| Date | Date the breach was detected. |
| Target | The target that was breached (for example, review SLA, escalation tier). |
| Owner | The role accountable for the missed target. |
| Detected by | Who recorded the breach. |
| Impact | Effect on users, contributors, or releases. |
| Root cause | Why the target was missed. |
| Remediation | Action taken to resolve the breach. |
| Preventive action | Change made to avoid a repeat. |
| Status | Open or Closed. |

## Log

| Breach ID | Date | Target | Owner | Impact | Root cause | Remediation | Status |
|-----------|------|--------|-------|--------|------------|-------------|--------|
| _none recorded_ | | | | | | | |

## Root Cause and Remediation

- Every breach gets a root cause written within **5 business days**.
- Remediation is assigned to an owner with a target date and tracked to closure.
- Repeat breaches of the same target trigger a review of the underlying process.

## Review Cadence

- Reviewed **monthly** by the maintainers.
- Summarized in the quarterly governance report.
- The log is append-only; closed entries are never deleted.

## Related Documents

- `Governance/policies/REVIEW_SLA.md`
- `docs/governance/matrices/escalation-matrix.md`
