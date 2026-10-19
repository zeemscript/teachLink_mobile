# Children's Data Policy

## Purpose

TeachLink Mobile is used in K-12 classrooms, so some of its users are children. This
policy sets the rules for handling their data: the age thresholds that apply, how
parental or school consent is obtained and recorded, and how much data may be collected
at all. It turns the commitments in the [Scope Statement](../SCOPE.md) and the
[Principles](../PRINCIPLES.md) ("verified consent for students under 13", "data
minimization") into rules that contributors and maintainers can apply and check.

> This policy is a governance baseline for the project. It is **not legal advice**.
> Where applicable law (for example COPPA, FERPA, GDPR, or a national children's code)
> is stricter than this policy, the law applies. The maintainers should have material
> changes reviewed by qualified counsel where the project has access to it.

## Scope

This policy applies to any data about a user who is, or may be, under the applicable
age of digital consent, wherever that data is collected, stored, processed, or sent:
in the app, in telemetry and crash reports, in third-party SDKs, in backend services
operated for the project, and in test data, screenshots, and bug reports.

It builds on, and does not replace, the [Privacy Manifest Policy](../domains/PRIVACY_MANIFEST.md),
the [Mobile Telemetry Governance](../domains/MOBILE_TELEMETRY.md), and the
[Mobile Permissions Policy](../domains/MOBILE_PERMISSIONS.md). Where this policy is
stricter for child accounts, it controls.

## Age Thresholds

| Band | Treatment |
| --- | --- |
| **Under 13** | A *child account*. Parental consent, or a school's authorisation where the law permits it for educational use, is required before any personal data is collected. The strictest rules in this policy apply |
| **13 up to the local age of digital consent** | In some jurisdictions the age of digital consent is above 13 (under GDPR it is set by each member state, between 13 and 16). In those places the account is treated as a child account until the local age is reached |
| **At or above the local age of digital consent** | An ordinary account, though data minimization still applies |

The app applies the **higher** of the global threshold and the local one. If a user's
age or location cannot be established, the account is treated as a child account until
it can. The maintainers record the thresholds they apply per region and review them at
least once a year, because the law changes.

## Account Model

TeachLink Mobile accounts for students are **teacher- or school-managed**: a teacher or
administrator creates the class and roster, and students do not register independently
(see the [Scope Statement](../SCOPE.md)). This policy depends on that model:

- A student account is created only through a roster action by an authorised adult.
- A child cannot self-register, change their own age band, or contact people outside
  their class.
- If a child account is found that was not created through a roster, it is suspended
  from collecting data until consent is confirmed.

## Parental Consent and School Authorisation

| Step | Rule |
| --- | --- |
| **Who consents** | A parent or guardian, or a school acting under its legal authority to authorise use for an educational purpose. The school's authorisation covers educational use only, not commercial use of the data |
| **Before collection** | No personal data of a child is collected before consent or authorisation is recorded. The app collects nothing from a child account except what is needed to show the consent request |
| **Verification** | The method must be reasonable for the risk and acceptable under the law that applies. The maintainers document the methods in use and the reason each is acceptable |
| **What is recorded** | Who consented, on whose behalf, the date and version of the notice shown, the scope of the consent, and how it was verified. The record holds no more personal data than that |
| **Scope** | Consent is specific to the purposes described in the notice. A new purpose needs new consent |
| **Withdrawal** | A parent, guardian, or the school can withdraw consent at any time, using a method at least as easy as giving it. Withdrawal stops collection immediately |
| **Deletion** | On withdrawal or request, the child's personal data is deleted within **30 days**, including from backups on their normal cycle, unless the law requires it to be kept. The deletion is confirmed to the requester |
| **Notice** | The notice is written in plain language for a parent, names the data collected and why, and is versioned with the published privacy policy it belongs to |

Consent records are kept for as long as the child's data is kept and for the minimum
period the law requires afterward, then deleted.

## Data Minimization

For child accounts, the project collects the **minimum** needed to deliver the
educational function, and nothing more. In particular:

- **Allowed** only where needed for the feature: the student's name or an alias chosen
  by the teacher, a class identifier, and the work the student submits.
- **Not collected at all:** precise location, contacts, the advertising identifier,
  photos or audio unless a teacher-assigned activity requires them, and biometric data.
- **No profiling or advertising.** Child data is never used for advertising, behavioural
  profiling, or sale, and is never shared with third parties for their own purposes.
- **Telemetry.** Analytics are off for child accounts unless limited to aggregate,
  non-identifying product-health data approved under the
  [Mobile Telemetry Governance](../domains/MOBILE_TELEMETRY.md). **Session replay is
  not used** on child accounts. Crash reports exclude personal data.
- **Third-party SDKs.** An SDK is permitted in a build that serves child accounts only
  if its data use is declared in the privacy manifest and it does not collect or
  transmit personal data of a child for its own purposes.
- **Retention.** Data is kept only as long as the educational purpose and the school's
  retention settings require, and then deleted.
- **Contact.** Messaging is limited to people in the child's class or school context, as
  controlled by the teacher. There is no public profile and no open search for children.

Test fixtures, screenshots, logs, bug reports, and issue attachments must never contain
real child data. Use synthetic data.

## Responsibilities

| Role | Responsibility |
| --- | --- |
| [Maintainers](../roles/MAINTAINER.md) | Own this policy; approve changes; decide the regional thresholds |
| [Security Officer](../roles/SECURITY_OFFICER.md) | Reviews changes that touch child data; leads incident response; confirms deletion mechanisms work |
| [Community Manager](../roles/COMMUNITY_MANAGER.md) | Keeps the parent-facing notice and the privacy policy accurate |
| Contributors | Do not add collection of child data without a privacy manifest change; use synthetic data |

## Reviewing Changes

A pull request that adds or changes data collection, an SDK, a permission, or a
telemetry event is checked against this policy when it affects an account that can be a
child's. The reviewer confirms that the privacy manifest is updated, that child accounts
are handled explicitly rather than by default, and that no collection is added that the
list above forbids. Such a change is not completable without that check.

## Incidents

A suspected exposure of a child's data is handled as a security incident under the
[Security Policy](../SECURITY_POLICY.md) and the [Escalation Path](../processes/ESCALATION_PATH.md),
with these additions: the Security Officer is notified at once; affected schools and
parents are told as soon as the facts allow and within any period the law sets; and the
post-incident review is published in redacted form.

## Related Documentation

- [Scope Statement](../SCOPE.md) and [Principles](../PRINCIPLES.md)
- [Privacy Manifest Policy](../domains/PRIVACY_MANIFEST.md)
- [Mobile Telemetry Governance](../domains/MOBILE_TELEMETRY.md)
- [Mobile Permissions Policy](../domains/MOBILE_PERMISSIONS.md)
- [Security Policy](../SECURITY_POLICY.md)
