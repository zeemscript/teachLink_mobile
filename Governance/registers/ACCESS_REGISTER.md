# Access Register

This register records who has which access to TeachLink Mobile systems and
resources. Its purpose is to keep access least-privileged and auditable by
tracking every grant, the reason for it, and its review and revocation.

## Who-Has-What-Access Fields

Each access record contains the following fields:

| Field | Description |
|-------|-------------|
| Access ID | Sequential identifier (for example, `ACCESS-0001`). |
| Person | Name or GitHub handle of the account holder. |
| Role | Role that justifies the access. |
| Resource | Repository, CI, store, or service the access applies to. |
| Access level | Read, write, admin, or a role-specific level. |
| Granted by | Who approved and granted the access. |
| Grant date | Date the access was granted. |
| Revoke date | Date the access was revoked, if applicable. |
| Last reviewed | Date of the most recent access review. |
| Status | Active, Revoked, or Expired. |

## Register

| Access ID | Person | Resource | Access level | Grant date | Revoke date | Status |
|-----------|--------|----------|--------------|-----------|-------------|--------|
| _none recorded_ | | | | | | |

## Grant and Revoke Dates

- Access is granted only after approval by a maintainer, and the Security Officer
  is informed for privileged resources.
- The grant date is recorded at the time access is created.
- When a role ends, access is revoked and the revoke date recorded promptly; see
  [`Governance/processes/OFFBOARDING.md`](../processes/OFFBOARDING.md).

## Periodic Review

- Access is reviewed **quarterly** by the Security Officer with the maintainers.
- Review confirms each grant is still required and matches the holder's role.
- Excess access is revoked and the action recorded in the register.
- Revoked records keep their entry and are never deleted.

## Related Documents

- `Governance/processes/OFFBOARDING.md`
- `Governance/roles/MAINTAINER.md`
