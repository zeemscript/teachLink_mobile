# Security Exceptions Register

This register records every approved exception to a security requirement or
policy. Its purpose is to make accepted risk explicit, time-bounded, and owned,
so exceptions are reviewed and closed rather than forgotten.

## Exception Fields

Each exception records the following fields:

| Field | Description |
|-------|-------------|
| Exception ID | Sequential identifier (for example, `SECEX-0001`). |
| Date | Date the exception was approved. |
| Requirement | The security requirement or policy being waived. |
| Justification | Why the exception is needed. |
| Compensating controls | Controls that reduce the accepted risk. |
| Residual risk | Risk that remains after the compensating controls. |
| Risk rating | Critical, High, Medium, or Low. |
| Approver | Security Officer and required maintainers. |
| Owner | Role accountable for the exception. |
| Expiry date | Date the exception lapses and must be renewed. |
| Status | Active, Expired, or Revoked. |

## Register

| Exception ID | Date | Requirement | Risk rating | Approver | Expiry date | Status |
|--------------|------|-------------|-------------|----------|-------------|--------|
| _none recorded_ | | | | | | |

## Compensating Controls

Every exception must name at least one compensating control that reduces the
residual risk. The control is specific, verifiable, and monitored by its owner.
An exception without a compensating control is not approved.

## Expiry and Review

- Each exception has an explicit expiry date, no more than **12 months** ahead.
- Exceptions are reviewed **quarterly** by the Security Officer with the maintainers.
- At expiry the exception is renewed with fresh justification or revoked.
- Expired and revoked exceptions keep their entry and are never deleted.

## Related Documents

- `Governance/SECURITY_POLICY.md`
- `Governance/registers/RISK_REGISTER.md`
