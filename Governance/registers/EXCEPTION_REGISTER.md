# Exception and Waiver Register

This register records exceptions and waivers granted against TeachLink Mobile
policies and processes. Its purpose is to make each exception explicit, owned,
and time-bounded, so that waivers are reviewed and closed rather than left open
indefinitely.

## Exception Fields

Each exception records the following fields:

| Field | Description |
|-------|-------------|
| Exception ID | Sequential identifier (for example, `EXC-0001`). |
| Date | Date the exception was granted. |
| Policy | The policy or requirement being waived. |
| Justification | Why the exception is needed. |
| Scope | What the exception covers and for how long. |
| Approver | Role or person that approved the exception. |
| Owner | Role accountable for the exception. |
| Expiry date | Date the exception lapses. |
| Review outcome | Result of the most recent review. |
| Status | Active, Expired, or Revoked. |

## Register

| Exception ID | Date | Policy | Approver | Expiry date | Status |
|--------------|------|--------|----------|-------------|--------|
| _none recorded_ | | | | | |

## Approver and Expiry

- Every exception names an approver with the authority to grant it, following
  [`Governance/policies/APPROVAL_REQUIREMENTS.md`](../policies/APPROVAL_REQUIREMENTS.md).
- Every exception has an explicit expiry date, no more than **12 months** ahead.
- An exception without an approver or expiry date is invalid and is closed.

## Review Process

- Exceptions are reviewed **quarterly** by the maintainers.
- At review, each exception is renewed with fresh justification or revoked.
- Expired and revoked exceptions keep their entry and are never deleted.
- Recurring exceptions trigger a review of the underlying policy.

## Related Documents

- `Governance/policies/APPROVAL_REQUIREMENTS.md`
- `Governance/registers/SECURITY_EXCEPTIONS_REGISTER.md`
