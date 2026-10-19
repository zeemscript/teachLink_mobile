# On-Call Coverage Matrix

This matrix defines the on-call coverage windows for TeachLink Mobile, the primary
and backup assignment for each window, and how coverage gaps are handled.

## Coverage Windows

All windows are stated in UTC. A window is "covered" when a primary and a backup
are both assigned before the rotation begins.

| Window | Days | Hours (UTC) | Primary | Backup |
|--------|------|-------------|---------|--------|
| Business hours | Mon–Fri | 08:00–18:00 | Maintainer on rotation | Secondary maintainer |
| Off hours | Mon–Fri | 18:00–08:00 | On-call maintainer | Secondary maintainer |
| Weekend | Sat–Sun | 00:00–24:00 | On-call maintainer | Secondary maintainer |

## Primary and Backup Assignment

- Rotation is **weekly** and published at least seven days in advance.
- The primary acknowledges new incidents first and is expected to respond within
  the response target in `Governance/policies/REVIEW_SLA.md`.
- The backup takes over automatically if the primary does not acknowledge within
  30 minutes, and covers planned absences.

## Gap Handling

- Newly identified coverage gaps are escalated to the security officer and the
  on-call coordinator.
- Gaps must be filled within one rotation cycle; unresolved gaps are recorded in
  the governance report.
- Uncovered critical incidents are handled by the escalation path in
  `Governance/processes/ESCALATION_PATH.md`.

## Review Cadence

Reviewed **quarterly** alongside the governance review, and after any rotation
change.
