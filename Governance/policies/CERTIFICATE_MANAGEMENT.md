# Certificate Management Policy

## Purpose

This policy defines how certificates used by TeachLink Mobile are issued, renewed,
monitored for expiry, and revoked when they should no longer be trusted. A certificate
that expires takes a feature down with it; one that stays trusted after it should have
been revoked is a way in. Both failures are preventable by treating certificates as
tracked assets with an owner and a date, rather than as configuration somebody renews
when they notice.

## Scope

This policy covers every certificate in the project's lifecycle:

| Certificate | Purpose | Where it lives |
| --- | --- | --- |
| iOS distribution certificate (`.cer` / `.p12`) | Signs the iOS app distributed through the App Store | EAS credential store |
| iOS provisioning profile (`.mobileprovision`) | Binds the distribution certificate to the App ID and devices | EAS credential store |
| TLS server certificates | Terminates HTTPS for the API and WebSocket endpoints | Infrastructure provider |
| Certificate pinning material | Backup pins for app-to-server trust | App configuration, per the [Encryption Policy](ENCRYPTION.md) |
| Client and mTLS certificates, where used | Authenticates CI or a service to an internal endpoint | Secret store |

It does not cover code-signing or commit-signing keys as such: the iOS distribution
certificate is included because it is a certificate, while the private key behind it
and the Android upload keystore are governed by the
[App Signing Key Policy](../domains/APP_SIGNING_KEYS.md) and the
[Key Management Policy](KEY_MANAGEMENT.md).

## Issuance Rules

Certificates are created from a controlled source so that there is always one record of
what was issued and by whom.

- **Issued for a stated purpose.** Every certificate records what it is for, which
  system consumes it, and the period it is valid for. A certificate issued "just in
  case" is not tracked and eventually becomes the one nobody recognised.
- **Shortest viable validity.** Validity is set to what the job needs. A server
  certificate valid for a year is preferred over one valid for ten, because the renewal
  process is then exercised regularly and revocation is quicker to complete.
- **Controlled issuers.** Certificates are issued from the project's own accounts or
  providers. A certificate obtained from a public free-CA service, or one whose private
  key was generated on an untrusted machine, is not accepted.
- **Private keys generated in the controlled environment.** The private key is created
  where it will be stored, or transferred by an approved secure channel, and never
  generated on a personal machine and moved.
- **No shared certificates.** Each environment and purpose has its own certificate. A
  staging certificate is not reused in production.
- **Recorded at issuance.** The certificate, its purpose, its issuer, its not-before and
  not-after dates, and its owner are entered in the certificate inventory below
  immediately, not when someone next thinks to update it.
- **Store approval required.** A new certificate on an Apple or Google identity, or on
  a production hostname, is issued only with the approval required by the
  [PR Approval Requirements](APPROVAL_REQUIREMENTS.md).

## Ownership

| Certificate group | Accountable owner | Responsible for performing |
| --- | --- | --- |
| iOS distribution certificate and provisioning profile | [Release Manager](../roles/RELEASE_MANAGER.md) | Performing renewal; releasing the new profile into EAS |
| TLS certificates for production endpoints | [Security Officer](../roles/SECURITY_OFFICER.md) with the infrastructure maintainer | Approving issuance; the infrastructure maintainer performs it |
| Pinning material | [Security Officer](../roles/SECURITY_OFFICER.md) | Approving a pin change, which requires two maintainers under the [CI/CD Governance Policy](CICD_GOVERNANCE.md) |
| Client and mTLS certificates for CI | [Security Officer](../roles/SECURITY_OFFICER.md) | Approving; the requesting maintainer performs it |
| Test and preview certificates | The maintainer running the preview environment | Performing renewal |

Rules that hold for every certificate:

- Ownership is a role, not a person, so the responsibility survives a handover.
- The owner is someone who can actually renew the certificate without needing the
  previous owner. A certificate whose only knowledgeable holder is one person is a
  single point of failure and is flagged for a second owner.
- Two people must be able to complete any renewal, matching the rule in the
  [Secret Rotation Policy](SECRET_ROTATION.md).
- A certificate with no identified owner is treated as unmanaged: it is renewed and
  given an owner, or revoked.

## Renewal Rules

Renewal starts early enough that expiry is never the reason a build fails.

| Requirement | Detail |
| --- | --- |
| Renewal lead time | Production certificates are renewed **at least 30 days** before `notAfter`, and 60 days before for anything Apple-issued, because profile regeneration is not always immediate |
| Renewal changes nothing else | A renewal replaces the certificate, not the identity. Changing the subject, the SANs, or the key alongside the renewal is a separate change with its own review |
| Provisioning profile follows the certificate | An iOS renewal regenerates the provisioning profile bound to the new certificate; renewing one without the other leaves the pair mismatched |
| Verification before promotion | A renewed certificate is validated by a `preview` build or a staging request before it is used in production, per the promotion path in the [EAS Build Governance](../domains/EAS_BUILD_GOVERNANCE.md) |
| Retain the previous certificate | The superseded certificate is kept, in the secure store only, until a production build or request has been accepted with the new one, so a failed renewal can be reversed without an outage |
| No reuse of the old private key | A renewal generates a fresh key. Reusing the previous private key defeats much of the purpose of renewing |
| Retired certificates are revoked | Once the successor is in production, the previous certificate is revoked at the issuer rather than merely deleted locally, per the [Key Management Policy](KEY_MANAGEMENT.md#revocation) |
| Record it | Each renewal records the date, the owner, the certificate identifier, the new expiry, and the verification result |

## Expiry Monitoring

Expiry is monitored so that renewal is a scheduled task rather than an outage. The
owner of each certificate group is responsible for the following:

| Control | Requirement |
| --- | --- |
| Expiry date recorded | Every certificate's `notAfter` is in the inventory, so the remaining life is visible without opening the certificate |
| Calendar entry | A renewal date is scheduled per certificate, with a reminder 14 days ahead of the renewal date and a second reminder at 30 days before `notAfter` |
| Monthly check | The owner confirms the calendar matches the actual certificates in the store. A certificate present in the store but absent from the inventory is treated as a finding |
| Automated expiry alert | Where the provider or platform offers expiry notification, it is enabled rather than relied upon alone, so that a rotation does not silence the alert |
| Escalation on approach | If a certificate is within 14 days of expiry and not renewed, the owner escalates through the [Escalation Path](../processes/ESCALATION_PATH.md) rather than waiting for it to lapse |
| Post-expiry check | After each renewal the owner confirms the old certificate no longer appears as current anywhere, including in the EAS credential list and in any local `.mobileprovision` left on a build machine |

An expired certificate fails in the least obvious way: an iOS distribution certificate
blocks submission with a message that does not always name expiry, and a lapsed TLS
certificate produces an untrusted-connection screen for users. Both are more expensive
than a calendar entry.

## Revocation

A certificate is revoked at its issuer immediately when:

- its private key is suspected or confirmed exposed;
- its validity period ends and it has no successor;
- it is issued to a person or machine that leaves the project, under
  [Contributor Offboarding](../processes/OFFBOARDING.md);
- it is found configured for a purpose it was not issued for; or
- a provider or store announces that it has been superseded.

Revocation removes trust at the source; deleting the local copy is not revocation. The
revocation is recorded with its date and reason, and where the certificate was a
signing identity, the exposure is reported through the
[Security Policy](../SECURITY_POLICY.md) and tracked in the
[Incident Log](../registers/INCIDENT_LOG.md).

## Certificate Inventory

| Certificate | Issuer | Purpose | Owner | `notAfter` | Status |
| --- | --- | --- | --- | --- | --- |
| iOS distribution certificate | Apple Developer | App Store distribution | [Release Manager](../roles/RELEASE_MANAGER.md) | Recorded in the EAS credential store | Managed |
| iOS provisioning profile | Apple Developer | Binds certificate to App ID | [Release Manager](../roles/RELEASE_MANAGER.md) | Regenerated with the certificate | Managed |
| Production TLS certificate | Infrastructure provider | `api.teachlink.com` | [Security Officer](../roles/SECURITY_OFFICER.md) | Provider-managed | Managed |
| Backup pin set | App configuration | App-to-server trust | [Security Officer](../roles/SECURITY_OFFICER.md) | Set with a documented expiry | Managed |

Where a certificate's expiry date is held by a provider rather than by the project, the
inventory records who to ask. Where a value is not yet known, the cell names the source
of truth instead of being left blank, because a blank cell is indistinguishable from an
unmanaged certificate.

## Enforcement Status

- Expiry monitoring is a human activity driven by the inventory and the calendar. No
  workflow currently fails on an approaching certificate expiry.
- Apple's platform enforces its own expiry: an expired distribution certificate blocks
  submission, and an expired profile blocks installation of the build it belongs to.
  This is a backstop, not a control; it is detected at the worst possible moment.
- Pinning material, if introduced, is the certificate with the largest expiry risk,
  because a missed rotation becomes an outage rather than a warning. A pin set without
  a backup pin is rejected under the [Encryption Policy](ENCRYPTION.md).

## Exceptions

A certificate that cannot be renewed within the lead time, or that must remain valid
past its intended expiry, is recorded in the
[Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md) with an approver
holding the authority to grant it, the compensating control, and an expiry no more than
12 months ahead. An exception to "renew before expiry" is rarely acceptable for a
signing certificate; for a test-environment certificate it may be.

## Ownership and Review

The Security Officer owns this policy, with the Release Manager responsible for the
Apple-issued certificates. It is reviewed when a new certificate type is introduced,
when a provider changes its issuance model, and at least once every 12 months. Changes
are proposed in a pull request that touches only the `Governance/` folder.

## Related Documentation

- [Key Management Policy](KEY_MANAGEMENT.md): the private key lifecycle behind each
  certificate
- [Secret Rotation Policy](SECRET_ROTATION.md): credential cadence and verification
- [Encryption Policy](ENCRYPTION.md): transport requirements and pinning rules
- [App Signing Key Policy](../domains/APP_SIGNING_KEYS.md): iOS and Android signing
  custody and promotion
- [EAS Build Governance](../domains/EAS_BUILD_GOVERNANCE.md): the promotion path a
  renewal follows
- [Contributor Offboarding](../processes/OFFBOARDING.md): revocation on departure
- [Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md)
