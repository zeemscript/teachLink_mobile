# Consent Management Policy

## Purpose

This policy defines what requires the user's consent, how consent is asked for and
recorded, how it is withdrawn, and what the app does when consent is absent. Consent is
easy to implement badly: a banner that appears once and is never revisited, a checkbox
pre-ticked, a withdrawal path that does not exist. This document makes each of those a
named failure.

## Scope

This policy covers consent for anything TeachLink Mobile does that is not strictly
necessary for the app to function, and it covers the collection and use of personal data
where the lawful basis is consent.

It covers:

- what is asked for, in the app and in the store listing;
- the wording, granularity, and presentation of each request;
- how consent is recorded and evidenced;
- how consent is withdrawn, and what happens then;
- consent obtained from a parent or guardian for a child; and
- how a change to what is consented to is handled.

It does not define what data is collected, which is the
[Privacy Manifest Policy](../domains/PRIVACY_MANIFEST.md), nor the telemetry rules, which
are the [Mobile Telemetry Governance](../domains/MOBILE_TELEMETRY.md). Where those
documents state a requirement, this policy states the mechanism for obtaining it.

## What Requires Consent

| Processing activity | Consent required | Notes |
| --- | --- | --- |
| Essential processing: authentication, account, core lesson delivery, payments | **No** | Necessary to provide the service; covered by the contract and the privacy policy rather than an in-app consent prompt |
| Analytics and product telemetry | **Yes**, opt-in | Must be off until consent is given; see [Mobile Telemetry Governance](../domains/MOBILE_TELEMETRY.md) |
| Crash reporting with personal data | **Yes** | Where the data would otherwise contain personal data, consent is required or the data is scrubbed |
| Push notifications | **Yes**, per category | Notification categories are consented to separately; consent for one does not imply another |
| Biometric authentication | **Yes**, explicit | Enabled only after the user turns it on, per the [Biometric Authentication Policy](../domains/BIOMETRIC_AUTH.md) |
| Optional device permissions | **Yes** | Requested in context, with the app's core function working if refused, per the [Mobile Permissions Policy](../domains/MOBILE_PERMISSIONS.md) |
| Personalisation that requires profiling | **Yes** | Must be separately consentable from analytics |
| Sharing data with a third party beyond the service providers | **Yes** | Each distinct recipient or purpose is a separate consent |
| Data processed in a new region or for a new purpose | **Yes**, renewed | A purpose change requires fresh consent, not the original consent extended |
| Research or analytics use of de-identified data | **Yes**, where the law requires it | Where the data is genuinely irreversible, the basis may be elsewhere; the choice is recorded |

Two rules govern the whole table:

- **Necessary processing is not disguised as consent.** A prompt that asks for something
  the app cannot work without is not consent, and presenting it as one is misleading.
- **Consent is per purpose, not per product.** "Yes, I agree to analytics" does not
  cover notifications, sharing, or a new purpose. Bundled consent is treated as invalid
  in this policy.

## How Consent Is Requested

A valid request meets all of these conditions.

| Condition | Requirement |
| --- | --- |
| Purpose stated plainly | The prompt says what data is used for, in the user's terms, not in the vendor's |
| Freely given | Refusing has no penalty. A refusal does not block the core app, reduce functionality, or degrade the experience for unrelated features |
| Unbundled | Each purpose is requested separately, not as one "Accept all" with no granular choice |
| Not pre-selected | The opt-in control starts off. A pre-ticked box, or a consent implied by not declining within a countdown, is not consent |
| Granular where it matters | Notification categories, analytics, and sharing are separable |
| Explicit action required | Consent follows an affirmative action, never the mere absence of objection |
| Accessible | Presented in a form a screen reader and a translator can handle, with alternatives to the prompt itself |
| Non-disruptive | Repeated requests for the same consent are not allowed; declining is remembered and not re-asked on every launch |
| Equal prominence | Accept and decline are visually comparable; a decline option is not hidden behind a link |

A prompt that fails any of these is changed rather than defended. In particular, a
"continue without accepting" path is never presented as the default or the recommended
option.

## How Consent Is Recorded

| Element | Requirement |
| --- | --- |
| Record content | Who consented, for which purpose, the wording version shown, the timestamp, and the mechanism used |
| Granularity | One record per purpose, so withdrawal of one purpose does not remove the others |
| Evidence | The record must show what the user actually saw, not merely that they agreed |
| Storage | Held in a controlled store with restricted access, per the [Encryption Policy](ENCRYPTION.md), and covered by the [Access Register](../registers/ACCESS_REGISTER.md) |
| Retention | Retained while the consent is relied on and for a defined period afterwards as evidence, per the [Record Retention and Legal Hold Policy](RECORD_RETENTION.md) |
| Children | Recorded against the parent or guardian who gave it, linked to the child's account per the [Children's Data Policy](../compliance/CHILDRENS_DATA.md) |
| Account transfer | A consent record does not transfer to a new account owner or a merged account; it is re-obtained |
| Access to records | A user may ask what is recorded and receives it, subject to the privacy rules in the [Privacy Policy Governance](../compliance/PRIVACY_POLICY_GOVERNANCE.md) |

Where consent cannot be evidenced, the project does not rely on it. An unrecorded
consent is treated as absent, and the activity stops.

## How Consent Is Withdrawn

Withdrawal must be as easy as giving consent, which for most purposes means inside the
app rather than only by contacting the project.

| Rule | Requirement |
| --- | --- |
| Access to withdraw | A single, discoverable control in the app's settings, and a written channel outside the app |
| Effect | From withdrawal, the consented activity stops. Data already collected is handled under the [Right-to-Erasure Policy](DATA_DELETION.md) |
| No penalty | Withdrawing does not reduce functionality, block access, or require a reason |
| Scope respected | Withdrawing one purpose leaves the others in place; withdrawing all consent ends the optional processing only, not the account |
| Propagated | Subprocessors are instructed to stop processing, per the [DPA Template](../compliance/DPA_TEMPLATE.md) |
| Confirmed | The app confirms that the withdrawal took effect and states what stopped |
| Essential data | Data necessary to provide the service is not deleted by withdrawing optional consent; the account is closed instead, following the [Right-to-Erasure Policy](DATA_DELETION.md) |

A user who withdraws consent for analytics and continues using the app is not asked why,
and analytics does not resume on the next update or prompt cycle.

## Consent for Children

Consent for a child follows the [Children's Data Policy](../compliance/CHILDRENS_DATA.md),
which this policy does not restate:

- parental or guardian consent is obtained through the age-appropriate flow the
  children's policy defines, verified to the standard that policy requires;
- the child's own assent is sought where the law or the children's policy requires it,
  and is recorded separately from the parent's consent;
- a child's consent cannot substitute for a parent's, and a parent's consent is not
  evidence of the child's assent;
- when consent cannot be obtained, the optional processing is off rather than assumed;
  and
- a change affecting children is reviewed under that policy's change-review process
  before it ships.

## Changing What Is Consented To

A change to the purposes, the wording, or the recipients covered by an existing consent
requires fresh consent.

- **Wording changes that narrow the request** continue under the original consent; the
  wording version is recorded so the evidence shows what was agreed.
- **Wording changes that broaden the request** require re-consent. Substituting broader
  language under an existing consent is not permitted.
- **A new purpose, recipient, or region** requires a new consent prompt, and the old
  consent is not reused for it. See the [Data Residency Policy](DATA_RESIDENCY.md) for
  the residency position.
- **Re-consent respects the withdrawal.** Someone who declined is not re-prompted by
  default; where re-prompting is necessary, it is a one-time prompt that honours a
  decline made in the previous cycle.
- **A new privacy policy version** is published before it takes effect, following the
  [Privacy Policy Governance](../compliance/PRIVACY_POLICY_GOVERNANCE.md), and the
  consent screen is updated to match it in the same release.

## Enforcement Status

- Consent behaviour in the app is covered by unit tests for the settings and telemetry
  paths and is verified in the Maestro flows in `maestro/`. There is no automated check
  that a new screen collecting data adds a consent prompt, so a new feature's consent
  requirement depends on review.
- Defaults are enforced by the code that gates telemetry on consent, not by review
  alone: an unconsented path sends nothing, per the
  [Mobile Telemetry Governance](../domains/MOBILE_TELEMETRY.md).
- The consent record store is a controlled system; its contents are covered by the
  [Access Register](../registers/ACCESS_REGISTER.md) review.

## Exceptions

An activity that proceeds without consent is an exception, recorded in the
[Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md) with the legal basis
relied on, the compensating control, an approver holding the authority to grant it, and
an expiry no more than 12 months ahead. "It improves our metrics" is not a basis.
Where the activity concerns children, the
[Children's Data Policy](../compliance/CHILDRENS_DATA.md) conditions apply and an
exception that conflicts with them cannot be granted.

## Ownership and Review

The [Security Officer](../roles/SECURITY_OFFICER.md) owns this policy, with the
[Community Manager](../roles/COMMUNITY_MANAGER.md) accountable for the wording users see
and the [Release Manager](../roles/RELEASE_MANAGER.md) for propagation to subprocessors.
It is reviewed whenever a new purpose, recipient, or SDK is added, and at least once
every 12 months. Changes are proposed in a pull request that touches only the
`Governance/` folder.

## Related Documentation

- [Privacy Manifest Policy](../domains/PRIVACY_MANIFEST.md): the single source of truth
  for what is collected
- [Mobile Telemetry Governance](../domains/MOBILE_TELEMETRY.md): telemetry consent and
  PII exclusion
- [Mobile Permissions Policy](../domains/MOBILE_PERMISSIONS.md) and
  [Biometric Authentication Policy](../domains/BIOMETRIC_AUTH.md)
- [Right-to-Erasure Policy](DATA_DELETION.md): deleting data after withdrawal
- [Record Retention and Legal Hold Policy](RECORD_RETENTION.md): how long consent records
  are kept
- [Children's Data Policy](../compliance/CHILDRENS_DATA.md): parental consent and age
  thresholds
- [Privacy Policy Governance](../compliance/PRIVACY_POLICY_GOVERNANCE.md) and
  [Subprocessor Register](../registers/SUBPROCESSOR_REGISTER.md)
- [Encryption Policy](ENCRYPTION.md) and
  [Access Register](../registers/ACCESS_REGISTER.md)
- [Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md)
