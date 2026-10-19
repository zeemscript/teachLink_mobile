# End-of-Life Policy

## Purpose

This policy defines how the project decides that a release line, a feature, a platform
version, or an integration has reached its end of life, how that decision is announced,
and what happens to the people still relying on it. End of life is a decision with
consequences for users, so it is made deliberately, with notice, and with a migration
path rather than by letting something lapse.

## Scope

This policy covers end of life for anything the project stops maintaining:

- **Release lines**, following the two-line support window in the
  [Security Policy](../SECURITY_POLICY.md#supported-versions);
- **Features and capabilities** removed from the app;
- **Platform and OS versions** no longer supported, following the
  [OS Version Support Policy](../domains/OS_VERSION_SUPPORT.md);
- **Integrations and API surfaces** the backend no longer serves; and
- **Governance documents and processes** that are retired.

It covers the decision, the announcement, and the transition. The day-to-day support
commitment made to users on a supported line is in the
[Support Policy](SUPPORT.md), and the rules that determine which versions are in scope
are in the [Compatibility and Support Window Policy](COMPATIBILITY.md).

## How End of Life Is Decided

End of life is a maintainer decision recorded in writing, not an automatic consequence
of a date passing.

| Trigger | Decision | Approver |
| --- | --- | --- |
| A release line falls outside the supported window | Automatic, following the [Security Policy](../SECURITY_POLICY.md#supported-versions) | [Release Manager](../roles/RELEASE_MANAGER.md) confirms |
| A feature's purpose is gone, or its maintenance cost exceeds its use | Proposed in an issue with usage evidence | Maintainers, by [lazy consensus](../processes/LAZY_CONSENSUS.md) |
| A dependency, OS, or platform can no longer be supported safely | Proposed in an issue citing the constraint | Maintainers, with the [Security Officer](../roles/SECURITY_OFFICER.md) |
| The project itself winds down | Proposed as a formal proposal | [Supermajority](SUPERMAJORITY.md), per the [Decision Rights Matrix](../matrices/DECISION_RIGHTS_MATRIX.md) |

Rules that shape the decision:

- **Evidence over instinct.** A feature is proposed for end of life with evidence of
  low use or high cost. Where the project has no usage data, the issue says so
  explicitly rather than asserting that a feature is unused.
- **Security is a reason, not a shortcut.** A security-driven end of life may use a
  shorter notice, following the [Deprecation Policy](DEPRECATION.md#notice-period), but
  it is still announced and still has a migration path.
- **No end of life by silence.** Something is not end of life because nobody has
  mentioned it for a year. If it is still shipped, it is still maintained, or it is
  brought to a decision explicitly.
- **A decision is reversible until it is announced.** Before announcement, a proposal
  can be withdrawn without ceremony. After announcement, reversing it means a new
  decision and a new announcement.

## Support Reduction Stages

End of life is a transition with stages, so that users and integrators get warning
before the capability disappears.

| Stage | What it means | Duration before the next stage | What the project does |
| --- | --- | --- | --- |
| **Active** | Fully maintained: fixes, security updates, and new work as planned | — | Normal development |
| **Maintenance** | Receives security and critical fixes only; no new features | At least 90 days | Defects still triaged; [support](SUPPORT.md) continues |
| **Deprecated** | Announced as ending, replacement named, no new work | At least **90 days** of notice, per the [Deprecation Policy](DEPRECATION.md) | Migration guidance published; in-app or release-note notice |
| **Frozen** | No further changes of any kind; exists only for existing users | At least 90 days before removal | Remains installed and working; only a security fix that cannot wait may change it |
| **End of life** | No support, no fixes, no updates | Indefinite | Removed from distribution; existing installs stop receiving updates |

Rules that apply across the stages:

- A stage may not be skipped. Frozen follows Deprecated, and Deprecated follows
  Maintenance, with the notice above, unless a documented emergency exception applies.
- Stage changes are recorded in the
  [Deprecation Register](../registers/DEPRECATION_REGISTER.md) with the date, the
  reason, and the approver.
- A user on a Frozen item is not left broken: if the platform or a provider removes
  something the app depends on, that is a critical defect and is fixed under the
  [Patch Management Policy](PATCH_MANAGEMENT.md) even during Frozen.
- Moving from Maintenance to Deprecated is the point at which users are asked to act.
  Earlier stages do not require migration work from anyone.

## Announcement

An end-of-life decision is announced through every channel that reaches the people
affected, in the earliest applicable form:

| Audience | Channel |
| --- | --- |
| Users of the app | Release notes, `CHANGELOG.md`, an in-app notice, and the store listing where the stage is user-visible |
| Integrators and downstream developers | A tracking issue, documentation, and the API deprecation notice |
| Maintainers | The [meeting cadence](../processes/MEETING_CADENCE.md) record and the release issue |
| Subprocessors affected by a change | Per the [Subprocessor Register](../registers/SUBPROCESSOR_REGISTER.md#change-notification-process) notice period |

Every announcement states the same five things:

1. what is ending;
2. who is affected;
3. the date or version of the final stage;
4. the replacement or migration path; and
5. what support remains available, and until when.

An announcement that does not include the migration path is incomplete. "We are
removing this" without "use this instead" transfers the whole cost of the decision to
the user.

## Migration Guidance

Migration guidance is written **before** the Deprecated stage begins, not alongside the
removal.

- Each end-of-life item has a named replacement, or an explicit statement that there is
  none and what users should do instead.
- For data, the guidance covers export and the retention position, per the
  [Record Retention and Legal Hold Policy](RECORD_RETENTION.md), so a user is not told
  to migrate data that will be deleted or to retain data they no longer have.
- For a platform or OS change, the guidance states the minimum supported version and
  how to check it, following the
  [OS Version Support Policy](../domains/OS_VERSION_SUPPORT.md).
- For an integration, the guidance states the replacement endpoint or the deprecation
  date, and links the API documentation.
- The guidance is published in the documentation, not only in a release note, so it
  remains findable after the announcement.

## Turning It Off

For a mobile app, end of life is enforced by the store rather than by the project. A
line that reaches end of life stops receiving updates from the store as well as from
the project, and new users are directed to the current version. Where the project's own
guidance differs from the store's enforcement, the project's guidance is corrected to
match reality rather than the reverse.

The final act of end of life for a release line is recorded in the
[Deprecation Register](../registers/DEPRECATION_REGISTER.md) and reflected in the
[Security Policy](../SECURITY_POLICY.md) supported-versions table, so that the two
documents do not drift.

## Exceptions

A notice period shortened below 90 days requires a documented reason — typically a
security exposure or a provider withdrawal — an approver holding the authority to grant
it, and an entry in the
[Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md) with an expiry no
more than 12 months ahead. Shortening the notice is permitted; skipping the
announcement is not.

## Enforcement Status

- The stage model is applied by maintainers when a deprecation is proposed. No
  automation enforces the 90-day intervals or the ordering of the stages.
- Store-side enforcement of minimum versions is the platform's, not the project's, and
  is outside the repository.
- The [Deprecation Register](../registers/DEPRECATION_REGISTER.md) is a maintained
  document; its value depends on being updated when a stage changes.

## Ownership and Review

The Release Manager owns this policy with the maintainers. It is reviewed after every
end-of-life decision, when the support window changes, and at least once every 12
months. Changes are proposed in a pull request that touches only the `Governance/`
folder.

## Related Documentation

- [Support Policy](SUPPORT.md): the support commitment during each stage
- [Compatibility and Support Window Policy](COMPATIBILITY.md): which versions are in
  scope
- [Deprecation Policy](DEPRECATION.md): notice period and removal criteria
- [Deprecation Register](../registers/DEPRECATION_REGISTER.md): the record of each
  deprecation and its stage
- [Security Policy](../SECURITY_POLICY.md): the supported release lines
- [OS Version Support Policy](../domains/OS_VERSION_SUPPORT.md): platform versions
- [Versioning Policy](VERSIONING.md): how versions and lines are numbered
- [Record Retention and Legal Hold Policy](RECORD_RETENTION.md): what happens to data
  at end of life
- [Release Cadence](../processes/RELEASE_CADENCE.md) and
  [Release Manager role](../roles/RELEASE_MANAGER.md)
