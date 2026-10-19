# Permissions Matrix

This matrix defines which roles may access which project resources and what each
role is permitted to do. It complements the role documents in `Governance/roles/`
and the approval rules in `Governance/policies/APPROVAL_REQUIREMENTS.md`.

## Roles

| Role | Description |
|------|-------------|
| Contributor | Anyone who opens issues or pull requests. |
| Reviewer | Reviews and approves pull requests within their area. |
| Committer | Merges approved changes within their area. |
| Maintainer | Owns repository settings, releases, and membership. |
| Security Officer | Owns security-sensitive resources and disclosures. |
| Release Manager | Owns release, signing, and store resources. |

## Permissions per Role

| Resource | Contributor | Reviewer | Committer | Maintainer |
|----------|-------------|----------|-----------|------------|
| Issues | Create, comment | Triage, label | Close, transfer | Close, transfer |
| Pull requests | Open | Approve | Merge | Merge |
| Repository settings | None | None | None | Admin |
| Branch protection rules | None | None | None | Admin |
| Secrets and signing keys | None | None | None | Admin |
| CI/CD configuration | None | None | Propose | Admin |
| Release branches | None | None | None | Manage |
| Team membership | None | None | None | Admin |

## Principles

- Least privilege: a role receives only the access needed to do its work.
- Separation of duties: no single role both authors and merges security-sensitive changes.
- Security Officer and Release Manager permissions are granted per resource, not repository-wide.

## Review Cadence

- Reviewed **bi-annually** by the maintainers as part of the access review.
- Reviewed immediately after any role change or offboarding.
- Exceptions are recorded in the access review runbook.

## Related Documents

- `Governance/roles/MAINTAINER.md`
- `Governance/roles/SECURITY_OFFICER.md`
- `Governance/policies/APPROVAL_REQUIREMENTS.md`
- `Governance/processes/OFFBOARDING.md`
