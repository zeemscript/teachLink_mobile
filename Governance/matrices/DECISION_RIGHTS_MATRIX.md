# Decision Rights Matrix

This matrix records who holds decision rights for each class of decision in
TeachLink Mobile. Its purpose is to make ownership explicit, so contributors know
where a decision belongs and what to do when the owner cannot decide.

## Decision Types

| Decision type | Description |
|---------------|-------------|
| Release | Approving and publishing a release to a store or channel. |
| Feature scope | Accepting, deferring, or rejecting a proposed feature. |
| Architecture | Changing shared structure, patterns, or public interfaces. |
| Security | Accepting risk or approving a security exception. |
| Dependency | Adding, upgrading, or removing a third-party dependency. |
| Governance policy | Changing a document under `Governance/`. |
| Role appointment | Appointing a maintainer or other named role. |
| Access | Granting or revoking repository, CI, or store access. |

## Who Decides

| Decision type | Decision owner | Required input |
|---------------|----------------|----------------|
| Release | Release Manager | Maintainers |
| Feature scope | Maintainers | Product and community feedback |
| Architecture | Maintainers | Reviewers and domain owners |
| Security | Security Officer | Maintainers |
| Dependency | Maintainers | Security Officer |
| Governance policy | Maintainers | Community review |
| Role appointment | Maintainers | Nomination and second |
| Access | Maintainers | Security Officer |

## Escalation Fallback

When the decision owner is unavailable or the decision is contested:

1. Raise the decision with the maintainers in a public issue.
2. If the maintainers cannot reach consensus, follow
   [`Governance/processes/CONFLICT_RESOLUTION.md`](../processes/CONFLICT_RESOLUTION.md).
3. A tie is broken by the process in
   [`Governance/policies/TIE_BREAKING.md`](../policies/TIE_BREAKING.md).
4. If the maintainers remain deadlocked, the decision is deferred until the next
   governance review and recorded with the unresolved options.

## Review Cadence

Reviewed **annually** by the maintainers and updated whenever a role or policy
changes the decision rights.

## Related Documents

- `Governance/processes/ESCALATION_PATH.md`
- `Governance/policies/APPROVAL_REQUIREMENTS.md`
