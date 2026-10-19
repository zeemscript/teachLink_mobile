# RACI Matrix

This matrix assigns, for each governance activity, who is **R**esponsible,
**A**ccountable, **C**onsulted, and **I**nformed. It removes ambiguity about who
does the work and who owns the outcome.

## Legend

- **R** — Responsible: does the work.
- **A** — Accountable: owns the outcome and signs off (exactly one per activity).
- **C** — Consulted: provides input before the decision.
- **I** — Informed: notified after the decision.

## Activities

| Activity | Contributor | Reviewer | Maintainer | Security Officer | Release Manager | Docs Lead |
|----------|-------------|----------|------------|------------------|-----------------|-----------|
| Open and triage issues | R | C | A | I | I | I |
| Implement a change | R | I | A | I | I | I |
| Review a pull request | I | R | A | C | I | I |
| Merge a change | I | C | R/A | C | I | I |
| Security disclosure | I | I | A | R | I | I |
| Cut a release | I | I | A | C | R | I |
| Update governance docs | I | C | A | C | I | R |
| Resolve a conduct report | I | I | A | C | I | I |
| Review access and permissions | I | I | R/A | C | I | I |

## How It Is Kept Current

- The matrix is reviewed **quarterly** as part of the governance review.
- Any new recurring activity must be added with a single accountable role.
- Changes are made by pull request with at least one maintainer approval.

## Related Documents

- `Governance/roles/MAINTAINER.md`
- `Governance/processes/ESCALATION_PATH.md`
- `Governance/policies/APPROVAL_REQUIREMENTS.md`
