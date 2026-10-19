# Service Accounts Policy

## Purpose

This policy defines how a machine identity in TeachLink Mobile is created, what it is
allowed to do, how it is reviewed, and how it is retired. A service account is a
credential that belongs to no person: it outlives the engineer who added it, it acts
without a human present, and nothing about it is obvious from reading the code that
uses it. Without a register and a review cycle, service accounts accumulate the
broadest permissions in a system and become the quietest way into it.

## Scope

This policy covers every non-human identity that authenticates to a system TeachLink
Mobile depends on, including:

- CI and automation identities: GitHub Apps, bot accounts, and the workflow tokens
  created by the `GITHUB_TOKEN` and its declared `permissions:`;
- cloud and platform service accounts used for build, release, and submission;
- API keys issued to integrations that are not tied to a person's account, including
  telemetry, crash reporting, analytics, and feature-flag providers; and
- signing and submission identities, whose material is additionally governed by the
  [App Signing Key Policy](../domains/APP_SIGNING_KEYS.md).

It does not cover a developer's personal credentials on their own machine, and it does
not cover a human maintainer's own account, which is governed by the
[Access Register](../registers/ACCESS_REGISTER.md) and the
[Privilege Revocation Policy](REVOCATION.md).

## Creation

No service account is created outside this process. The process is deliberately
frictional: a machine identity created in a hurry is a machine identity nobody will
later be able to explain.

1. **Open a request.** An issue states the name of the identity, the system it will
   authenticate to, the job it performs, and the alternative considered — why a person
   cannot do this instead.
2. **Justify it.** The requester explains why the work cannot run under an existing
   identity. "The code needs to do it automatically" is not sufficient on its own; a
   single-purpose account is preferred over reusing a broad one.
3. **Name an owner.** A named maintainer is accountable for the account for as long as
   it exists. An account without an owner is not created.
4. **Request the narrowest scope.** The request lists the exact permissions, resources,
   and environments required, following the least-privilege rule below.
5. **Approve.** A maintainer approves, and the Security Officer approves any identity
   that can write to a repository, publish a release, read production data, or act on
   a store account.
6. **Create it in the approved store.** The credential is created in the platform's
   secret store or CI secret store, never in a shell history, a local file that will be
   committed, or a ticket.
7. **Record it.** The account is added to the
   [Access Register](../registers/ACCESS_REGISTER.md) and, where it processes personal
   data, to the [Data Processing Register](../registers/DATA_PROCESSING_REGISTER.md),
   in the same pull request that creates it.
8. **Verify and schedule.** The owner confirms the identity works with exactly the
   permissions granted, adds it to the rotation calendar in the
   [Secret Rotation Policy](SECRET_ROTATION.md), and sets its review date.

An identity created outside this process is treated as unmanaged: it is inventoried,
rotated, given an owner, or decommissioned at the next review. Nobody is second-guessed
for creating one in an incident, but it is not left undocumented afterwards.

## Least-Privilege Rule

Every service account receives the minimum set of permissions that lets it perform its
declared job, and nothing beyond it.

- **One job per account.** An account that both uploads builds and reads production
  logs is two accounts. Narrow scope that limits the blast radius of a leaked
  credential is worth the extra setup cost.
- **Environment scope, not ambient scope.** A credential is scoped to the single
  workflow, project, or environment that needs it. A secret defined for all workflows
  is available to every job that runs, which is a scope nobody chose deliberately.
- **Read over write.** Write is requested only where the job mutates state. No
  account is granted delete unless deletion is the job.
- **No standing production data access.** Production and personal data access is
  granted per job, through a short-lived credential, rather than as a long-lived
  account that is always able to read.
- **Short-lived where the provider supports it.** An expiring token that is renewed
  automatically is preferred over a permanent credential, because it limits the value
  of a leak and removes the need for much of the rotation cadence.
- **No impersonation of people.** An account may not act as a named human, and its
  activity must be attributable to itself in logs, so that "who did this" has an
  answer that is not "the token was shared".
- **Separate environments.** Production credentials are never available to a
  development, preview, or fork pull-request build. `EXPO_TOKEN` is available to
  `ci.yml`, `build-native.yml`, and `release.yml`, which is deliberate, and no
  non-release workflow may gain it to run a preview.
- **No shared credentials between people.** Each person's local credential is their
  own, per the [App Signing Key Policy](../domains/APP_SIGNING_KEYS.md).

A permission that cannot yet be justified is not granted speculatively. It is added
when a real requirement appears, as its own change with its own review.

## Usage Rules

- Credentials are read from the environment or the platform's credential API at runtime.
  They are never written into source, committed, printed, or included in build logs
  or artifacts that leave the maintainer team.
- Service-account tokens are not used for interactive logins, and are not shared in
  issues, pull requests, or chat.
- Activity by a service account is logged, and a log or trace that names the account
  is preferred where the provider supports it. An account used from an unexpected
  origin or at an unexpected time is treated as a suspected compromise under the
  [Security Policy](../SECURITY_POLICY.md).
- Automated work that opens pull requests follows the
  [Bot and Automation Governance Policy](BOT_AUTOMATION.md), including the rule that
  automation never approves its own changes.

## Review

| Aspect | Requirement |
| --- | --- |
| Cadence | Every service account is reviewed **quarterly**, alongside the [Access Register](../registers/ACCESS_REGISTER.md) review |
| Reviewer | The [Security Officer](../roles/SECURITY_OFFICER.md) with each account's named owner |
| Confirms | The account is still in use, its permissions still match its declared job, its owner is still correct, and its credentials are within their rotation age |
| Outcome | Re-approved, narrowed, or decommissioned; every outcome is recorded in the register |
| New accounts | Reviewed once at the end of the quarter in which they were created |

A quarterly review that finds an account nobody recognises is treated as a finding
against the process, not only against the account: the review records how it came to
exist and what allowed it to go unnoticed. An account with no activity and no owner is
decommissioned rather than retained "in case it is needed".

## Decommissioning

An account is retired when its job ends, when a replacement account takes over, when it
has had no use for two consecutive quarters, or when its permissions no longer have a
justification.

1. **Confirm it is no longer needed.** The owner confirms nothing in the repository,
     workflow, or provider configuration references the account.
2. **Revoke at the source.** The credential is revoked or deleted at the provider
     first. Rotating it to an unused value and leaving it valid is not revocation.
3. **Remove the references.** Workflows, scripts, environment files, and
     documentation stop referring to it, in the same change where practical so the
     repository never references a credential that does not exist.
4. **Rotate anything shared.** If the account used a shared secret, that secret is
     rotated as part of decommissioning, since the departing account held it.
5. **Record it.** The [Access Register](../registers/ACCESS_REGISTER.md) entry moves to
   `Revoked` with the revoke date. Entries are never deleted, so the register shows
     both current and historical identities.
6. **Confirm removal.** The Security Officer confirms at the next review that the
     revoked identity can no longer authenticate, and that the credential is not
     recoverable from build caches, artifacts, or forks.

Decommissioning is part of offboarding when the reason is a person leaving, under
[Contributor Offboarding](../processes/OFFBOARDING.md), and it is not deferred to a
later cleanup.

## Enforcement status

- No automated check detects an unused or over-permissioned service account. Quarterly
  review is a human activity, tracked through the register.
- The repository does not yet contain a dedicated service-account register; accounts
  are held in the [Access Register](../registers/ACCESS_REGISTER.md), which is why this
  policy directs every account there rather than to a second list that would drift.

## Exceptions

An account that must hold broader permissions than the least-privilege rule allows is
recorded in the [Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md)
with the specific permissions, the compensating control, an approver with authority,
and an expiry date no more than 12 months ahead. A blanket exemption for a class of
accounts is not an exception; it is a policy change proposed through the normal
governance process.

## Ownership and Review

The Security Officer owns this policy. It is reviewed when a new category of service
account appears, and at least once every 12 months. Changes are proposed in a pull
request that touches only the `Governance/` folder.

## Related Documentation

- [Access Register](../registers/ACCESS_REGISTER.md): the register every account is
  recorded in
- [Secret Rotation Policy](SECRET_ROTATION.md): credential lifetime and verification
- [CI/CD Governance Policy](CICD_GOVERNANCE.md): workflow permissions and secrets
- [App Signing Key Policy](../domains/APP_SIGNING_KEYS.md): signing and submission
  identities
- [Bot and Automation Governance Policy](BOT_AUTOMATION.md): automation that acts in the
  project
- [Contributor Offboarding](../processes/OFFBOARDING.md): retirement on departure
- [Data Processing Register](../registers/DATA_PROCESSING_REGISTER.md) and
  [Subprocessor Register](../registers/SUBPROCESSOR_REGISTER.md): accounts that handle
  personal data
- [Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md)
- [Security Officer role](../roles/SECURITY_OFFICER.md)
