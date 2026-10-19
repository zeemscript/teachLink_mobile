# Secret Rotation Policy

## Purpose

This policy defines how often each secret used by TeachLink Mobile is rotated, who is
accountable for rotating it, and what evidence proves the rotation happened. A secret
that is never rotated has no expiry, so a leaked credential stays useful to whoever
found it for as long as it is left in place. Rotation is therefore a scheduled
activity with a named owner, not a reaction to an incident.

## Scope

This policy covers every credential that authenticates TeachLink Mobile to an external
service, and every credential stored in a secret store or as a CI secret:

- GitHub Actions secrets, including `EXPO_TOKEN`, `MAESTRO_API_KEY`,
  `LHCI_GITHUB_APP_TOKEN`, and the workflow-scoped `GITHUB_TOKEN` permissions;
- EAS and Expo account credentials;
- App Store Connect and Google Play API credentials used for submission;
- third-party service API keys used by telemetry, crash reporting, or analytics;
- service-account credentials, under the
  [Service Accounts Policy](SERVICE_ACCOUNTS.md); and
- signing material and certificates, whose rotation is owned by the
  [App Signing Key Policy](../domains/APP_SIGNING_KEYS.md) and the
  [Certificate Management Policy](CERTIFICATE_MANAGEMENT.md).

Client configuration exposed to the app, such as `EXPO_PUBLIC_*` values declared in
`env.d.ts`, is out of scope. Those are not secrets: they ship inside the binary. This
policy requires that they hold no credential, which is why no API key may be added
under an `EXPO_PUBLIC_` prefix.

## Rotation Cadence by Secret Type

| Secret type | Examples | Maximum age between rotations | Triggered earlier by |
| --- | --- | --- | --- |
| Long-lived platform tokens | `EXPO_TOKEN`, App Store Connect API keys, Google Play service-account keys | **90 days** | Suspected exposure, a person leaving the role that uses it, a change of owner |
| Service-account credentials | Any machine identity under [Service Accounts Policy](SERVICE_ACCOUNTS.md) | **90 days** | Suspected exposure, decommission, privilege change |
| Third-party API keys | Telemetry, crash reporting, analytics, feature-flag providers | **180 days** | Suspected exposure, a subprocessor change, a provider's own expiry notice |
| CI integration tokens | `LHCI_GITHUB_APP_TOKEN`, Lighthouse and monitoring integrations | **180 days** | Suspected exposure, provider-side revocation |
| Store signing and submission credentials | `.p8` App Store Connect keys, Android upload keystore | Per the [App Signing Key Policy](../domains/APP_SIGNING_KEYS.md) | Suspected exposure, app signing identity change |
| TLS and API certificates | Certificates presented by services the app calls | Per the [Certificate Management Policy](CERTIFICATE_MANAGEMENT.md) | Provider expiry, trust-store change |
| Local developer credentials | A developer's own token on their own machine | At the developer's discretion | The developer leaving the project |

`GITHUB_TOKEN` is not rotated by hand. It is issued per job from the workflow's
declared `permissions:` and dies with the job; the control that matters is that its
permissions stay narrow, which is governed by the
[CI/CD Governance Policy](CICD_GOVERNANCE.md).

Rotation on a schedule is a ceiling, not a target. A secret exposed in a log, a
screenshot, a commit, a build artifact, or an issue is rotated **immediately**, before
the investigation is finished and regardless of when it was last rotated.

## Responsible Owner

Each secret has exactly one accountable owner. The owner is a role, not a person, so
that the responsibility survives a change of holder.

| Secret group | Accountable owner | Responsible for performing |
| --- | --- | --- |
| `EXPO_TOKEN` and EAS build credentials | [Release Manager](../roles/RELEASE_MANAGER.md) | Performing the rotation |
| `GITHUB_TOKEN` permissions, workflow secret definitions | [Security Officer](../roles/SECURITY_OFFICER.md) | Approving the permission change; the Release Manager performs it |
| Store submission API keys | [Release Manager](../roles/RELEASE_MANAGER.md) | Performing the rotation, with the Security Officer approving |
| Telemetry and analytics API keys | [Maintainer](../roles/MAINTAINER.md) owning that area | Performing the rotation |
| Service-account credentials | [Security Officer](../roles/SECURITY_OFFICER.md) | Approving; the account's named maintainer performs it |
| Third-party and CI integration tokens | [Security Officer](../roles/SECURITY_OFFICER.md) | Approving; the integration's maintainer performs it |

Rules that hold for every secret:

- The owner who rotates a secret is not the only person who can. Two maintainers must
  be able to perform every rotation, so that the absence of one person cannot block a
  rotation.
- No secret is owned by an individual account that cannot be transferred. Ownership
  follows the role, and the hand-over happens in
  [offboarding](../processes/OFFBOARDING.md).
- A secret with no identified owner is treated as unmanaged and is rotated
  immediately, then given an owner.

## Verification Requirement

A rotation is complete only when it has been verified. Creating a new secret is half
the work; the other half is confirming the old one no longer works and the new one
does, in both directions.

After every rotation, the owner confirms:

1. **The new credential works.** The consuming system authenticates with it. For a CI
   secret, the next run of the workflow that uses it succeeds. For a store key, a
   submission or credential-lookup call succeeds.
2. **The old credential no longer works.** The previous value is revoked at the
   provider, not merely replaced in the secret store. Verification here is an actual
   attempt with the old value that is expected to fail. A provider that cannot revoke
   a credential individually is recorded as a limitation.
3. **The old credential was used nowhere it should have been.** CI logs for the window
   since the last rotation are read for unexpected use of the retired credential, and
   for any output that printed a credential value.
4. **Every consumer was updated.** Each place the secret is referenced was changed, so
   no workflow or script is left holding a dead value. Consumers are found by searching
   the repository for the secret's name, not its value.
5. **The evidence is recorded.** The rotation is recorded in the issue or pull request
   that performed it, with the date, the owner, the secret name, the verification
   result for both old and new values, and any provider limitation found.

A rotation with no recorded verification is treated as not performed. Where evidence
cannot reasonably be retained in full, for example because the confirmation would
require storing the credential, the fact and method of verification are recorded
instead.

### Incident rotation

Where a secret is suspected exposed, the verification above is done in one direction
only: the new credential must work, and the old one is revoked first. Time spent
verifying an already-suspected credential is time an attacker still holds it, so the
order is revoke, then replace, then verify. The exposure is reported through the
[Security Policy](../SECURITY_POLICY.md) and tracked as an incident in the
[Incident Log](../registers/INCIDENT_LOG.md).

## Scheduling and Reminders

- The Security Officer maintains a rotation calendar covering every secret group in the
  table above, with the next due date for each.
- The calendar is reviewed at the monthly governance review. A secret that comes due
  without an owner present to rotate it is escalated through the
  [Escalation Path](../processes/ESCALATION_PATH.md).
- Calendar entries carry a reminder two weeks before the deadline, so a rotation that
  needs a coordinated maintenance window has time to be scheduled.
- A secret added after this policy was written is added to the calendar in the same
  pull request that introduces it. A secret with no calendar entry does not enter
  production.

## Enforcement status

- No workflow currently checks secret age, and no automation fails a build on an
  overdue rotation. The calendar is a maintained document, not a machine-enforced
  control, and a maintainer checks it at the monthly review.
- Automated secret scanning is a dependency-time concern rather than a secret-lifetime
  one; dependency advisories are handled by `audit.yml` and are out of scope here.
- The control this policy can most plausibly automate is age-based alerting on the
  platform credential store. Until that exists, an overdue rotation is a governance
  follow-up, not a build failure.

## Exceptions

A secret that cannot be rotated on the cadence above, for example because the provider
issues only annual credentials, is recorded in the
[Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md) with the reason,
the compensating control, an approver with authority to grant it, and an expiry date no
more than 12 months ahead. An exception is not a reason to rotate less often than the
provider allows; it records the gap between this policy and reality so that the gap is
visible.

## Ownership and Review

The Security Officer owns this policy and the rotation calendar. It is reviewed when a
new secret is introduced, when a provider changes its credential model, and at least
once every 12 months. Changes are proposed in a pull request that touches only the
`Governance/` folder.

## Related Documentation

- [CI/CD Governance Policy](CICD_GOVERNANCE.md): where secrets may be used and what
  permissions surround them
- [Service Accounts Policy](SERVICE_ACCOUNTS.md): machine identities whose credentials
  are rotated here
- [Certificate Management Policy](CERTIFICATE_MANAGEMENT.md): certificates and their
  renewal
- [App Signing Key Policy](../domains/APP_SIGNING_KEYS.md): signing material and the
  exposure path
- [Encryption Policy](ENCRYPTION.md): what the secrets protect and with which
  algorithms
- [Key Management Policy](KEY_MANAGEMENT.md): the wider key lifecycle this cadence sits
  inside
- [Security Officer role](../roles/SECURITY_OFFICER.md) and
  [Release Manager role](../roles/RELEASE_MANAGER.md)
- [Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md) and
  [Incident Log](../registers/INCIDENT_LOG.md)
