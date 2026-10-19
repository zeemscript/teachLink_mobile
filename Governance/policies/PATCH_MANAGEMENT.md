# Patch Management Policy

## Purpose

This policy defines how patches are brought into TeachLink Mobile, how they are
prioritised against other work, and how an emergency patch is delivered faster than the
normal release process. A patch process that only describes the routine case leaves the
project improvising exactly when a patch matters most, so the routine cadence, the
prioritisation rules, and the emergency path are all specified here.

## Scope

This policy covers every update that changes shipped code or a shipped dependency:

- application source changes that fix a defect or a security issue;
- dependency and transitive-dependency updates, driven by Renovate and by `audit.yml`;
- base image, toolchain, and build-dependency updates, including Expo SDK and React
  Native upgrades;
- Android and iOS platform dependency updates shipped with the app; and
- patches to vendored or generated code committed to the repository.

It does not govern the content of a security remediation deadline, which is the
[Vulnerability Remediation SLA Policy](VULN_SLA.md), nor the mechanics of producing and
signing a build, which are in the
[EAS Build Governance](../domains/EAS_BUILD_GOVERNANCE.md) and the
[App Signing Key Policy](../domains/APP_SIGNING_KEYS.md).

## Patch Cadence

| Patch class | Target window | Delivery |
| --- | --- | --- |
| Security patch, Critical or High severity | Per the [remediation SLA](VULN_SLA.md#remediation-targets-by-severity) | Expedited release, outside the normal cadence |
| Security patch, Medium or Low severity | 30 and 90 calendar days respectively | Next regular release |
| Routine dependency update, no advisory | Within **14 days** of the update becoming available, or in the next release, whichever is sooner | Next regular release |
| Security-adjacent tooling update, such as a CI action | Within **7 days** | Next release; immediately if it fixes an exploitable workflow flaw |
| Base image or toolchain patch | Within **30 days** | Next release, with build verification |
| Major version or framework upgrade | Planned work, not a patch | Milestone, following the [Versioning Policy](VERSIONING.md) |
| Platform SDK adoption | Planned work | Milestone, following the [OS Version Support Policy](../domains/OS_VERSION_SUPPORT.md) |

Two rules apply across all classes:

- **A patch is not a feature.** A patch lands on its own, is reviewed on its own, and is
  not bundled with unrelated changes. Bundling makes a patch unreviewable, which is the
  main reason bundled hotfixes take longer than they should.
- **Verification is part of the patch.** A patch ships only after the checks in
  [PR Approval Requirements](APPROVAL_REQUIREMENTS.md#required-checks) pass. A patch
  that cannot pass the checks is escalated, not merged with the checks relaxed.

## Prioritisation

When more patches are available than the release can carry, they are ordered by this
rule set, applied in order.

| Priority | Criterion | Example |
| --- | --- | --- |
| 1 | Actively exploited, or a flaw in the release-signing or update-delivery chain | Compromised signing credential; an advisory with a known exploit in the wild |
| 2 | Critical or High severity affecting users' data, accounts, or payment | Authentication bypass; token exposure |
| 3 | Severity for which no workaround or mitigation is available to users | A crash or data-loss defect on the current release line |
| 4 | Severity for which a workaround exists | Medium severity with a configuration mitigation |
| 5 | Patch that removes an exception, or closes a security finding | Clearing a recorded `security` finding |
| 6 | Routine dependency update removing a deprecated or vulnerable package | Removing a package flagged by `audit.yml` |
| 7 | Toolchain or build-dependency update with no user-visible effect | Developer-experience and build-stability updates |

Rules that resolve ties and edge cases:

- **User exposure outranks internal risk.** A defect affecting a teacher's data ranks
  above a defect affecting only maintainer tooling, at the same severity.
- **Reachability decides, not the advisory.** An advisory for a code path the app never
  reaches is not automatically low priority; it is first assessed for reachability, and
  the assessment is recorded. Severity is only lowered with a written argument, which is
  the adjustment the [Vulnerability Remediation SLA Policy](VULN_SLA.md) describes.
- **Unblocking counts.** A patch that unblocks a release or removes a flakiness that is
  consuming maintainer time moves up.
- **A patch is never deprioritised for being inconvenient.** "Large diff", "touching
  many files", and "risky to merge" are reasons to review carefully and split into
  reviewable pieces, not reasons to postpone indefinitely.
- **Batch size is bounded.** A routine update release carries a limited number of
  changes, so that a regression can be attributed. When the limit is reached, the
  remainder waits for the next release rather than growing an unreviewable batch.

## Emergency-Patch Path

The emergency path exists for a defect or security issue that cannot wait for the normal
release cadence. It shortens the process; it does not remove the controls.

1. **Declare.** The Release Manager or Security Officer declares the emergency, states
   the user impact, and opens an issue labelled `emergency`.
2. **Scope minimally.** The patch contains only what is needed to resolve the emergency.
   Additional improvements wait for the next release, which is what keeps the patch
   reviewable.
3. **Fix on a dedicated branch.** The work happens on a branch from the current release
   tag or `main`, so it does not inherit unrelated in-flight changes.
4. **Review.** Two maintainers review, at least one of whom is the Release Manager. For
   a security fix the Security Officer reviews as well. The emergency override in
   [PR Approval Requirements](APPROVAL_REQUIREMENTS.md#emergency-overrides) allows
   fewer checks, never fewer approvals, and the reason is recorded.
5. **Verify locally as far as the emergency allows.** The full suite is still run; where
   a non-blocking check cannot complete in time, it is recorded and re-run afterwards
   rather than assumed.
6. **Ship.** The build follows the promotion path in the
   [EAS Build Governance](../domains/EAS_BUILD_GOVERNANCE.md): `development`, then
   `preview` with a real device, then `production`. A signed `preview` build is
   confirmed before promotion.
7. **Recover.** Within 24 hours the full test suite is run against the shipped change.
   Where it fails, the emergency patch is reverted on the next release and the cause is
   recorded, following the override's post-merge requirements.
8. **Follow up.** A post-incident review is filed for the cause and for anything the
   emergency deferred, so the gap is closed rather than remembered.

An emergency patch that changes the release-signing chain additionally follows the
[App Signing Key Policy](../domains/APP_SIGNING_KEYS.md), including the requirement to
validate with a `preview` build before a production submission.

Emergency status is used for user impact, not for a missed internal deadline. If it is
being used for the second thing, the deadline is the problem to fix, and that is
recorded as a governance item.

## Release Cadence Relationship

Patches flow through the
[Release Cadence](../processes/RELEASE_CADENCE.md) and the
[Release Branching Policy](RELEASE_BRANCHING.md). A regular release draws from
`main` and carries the routine batch; an expedited release is cut from the emergency
branch and is reconciled into `main` afterwards, so `main` always reflects what shipped.
Every patch appears in `CHANGELOG.md` and the release notes under the
[Changelog Policy](CHANGELOG_POLICY.md), with the issue it resolves referenced.

## Verification

| Check | Requirement |
| --- | --- |
| Checks pass | The required checks pass on the patch, or a recorded exception applies |
| No unrelated change | The diff contains only the patch, as reviewers confirm |
| Reproduces and fixes | Where the defect is reproducible, the patch is shown to fix it and not to change the behaviour under test |
| Rehearsed on a real build | For a platform or dependency patch, a `preview` build installs and launches |
| Attributable | The change is one patch per release entry, so a regression can be traced to it |
| Reverted cleanly if wrong | A revert is possible without a coordinated rollback, which is checked before shipping |

## Enforcement Status

- `audit.yml` runs on push and weekly and fails on high and critical dependency
  advisories; Renovate proposes routine updates. This is the automated part of the
  cadence.
- The 14-day routine window, the prioritisation order, and the emergency declaration
  are human processes. No automation enforces a patch age or the batch-size bound.
- Reverting is deliberately easy because patches land on their own branches; that is a
  design property, not a control.

## Exceptions

A patch that cannot be applied within its window — a dependency that breaks the build,
a vendor release that regresses a tested path, a platform fix unavailable to the project
— is recorded in the
[Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md) with the reason,
the compensating control, an approver with authority, and an expiry no more than 12
months ahead. An exception says why a patch is late; it does not make the late patch
optional, and the patch is still applied as soon as the blocker is resolved.

## Ownership and Review

The Release Manager owns this policy, with the Security Officer responsible for the
security-patch classes. It is reviewed after every emergency patch, when the release
cadence changes, and at least once every 12 months. Changes are proposed in a pull
request that touches only the `Governance/` folder.

## Related Documentation

- [Vulnerability Remediation SLA Policy](VULN_SLA.md): remediation deadlines by severity
- [Release Cadence](../processes/RELEASE_CADENCE.md) and
  [Release Branching Policy](RELEASE_BRANCHING.md): how patches reach users
- [PR Approval Requirements](APPROVAL_REQUIREMENTS.md): checks and the emergency
  override
- [EAS Build Governance](../domains/EAS_BUILD_GOVERNANCE.md): the promotion path
- [App Signing Key Policy](../domains/APP_SIGNING_KEYS.md): patches that touch signing
- [Versioning Policy](VERSIONING.md): where a major upgrade sits relative to a patch
- [Bot and Automation Governance Policy](BOT_AUTOMATION.md): Renovate and automated
  updates
- [Dependency Register](../registers/DEPENDENCY_REGISTER.md) and
  [Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md)
- [Release Manager role](../roles/RELEASE_MANAGER.md)
