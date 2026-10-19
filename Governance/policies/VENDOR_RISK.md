# Vendor Risk Management Policy

## Purpose

This policy defines how a third-party service or dependency is assessed before the
project relies on it, how its risk is tiered, and how that risk is reviewed over time.
Most of the software TeachLink Mobile runs is written by someone else, so the project's
security posture is largely the aggregate of its vendors' postures.

## Scope

This policy covers every third party whose failure or compromise would affect
TeachLink Mobile:

- services the app calls at runtime, including the API host, authentication, push
  notifications, analytics, crash reporting, and AI services;
- build and distribution services, including Expo Application Services, GitHub Actions,
  and the CI providers in `.github/workflows/`;
- app stores and payment providers;
- libraries and packages, which are additionally covered by the
  [Dependency Register](../registers/DEPENDENCY_REGISTER.md); and
- infrastructure and hosting, including anything that stores data.

Processors that handle personal data are additionally governed by the
[DPA Template](../compliance/DPA_TEMPLATE.md) and recorded in the
[Subprocessor Register](../registers/SUBPROCESSOR_REGISTER.md). This policy decides
whether a vendor is acceptable and how that acceptance is kept current.

## Vetting Criteria

A vendor is assessed against these criteria before it is relied upon. The assessment is
recorded, not merely performed, so that a later reader can see why the vendor was
accepted.

| Criterion | Question | Evidence |
| --- | --- | --- |
| Purpose and necessity | What does it do, and could the project do without it? | A stated purpose; the alternative considered |
| Data handled | What data reaches it, including metadata and content? | The categories listed in the [Data Processing Register](../registers/DATA_PROCESSING_REGISTER.md) |
| Security posture | Does it encrypt data, manage access, and disclose incidents? | Public documentation, certifications where relevant, and the answers to a security questionnaire |
| Subprocessors | Who else receives the data? | The vendor's own subprocessor list, recorded in the [Subprocessor Register](../registers/SUBPROCESSOR_REGISTER.md) |
| Data location | Where is the data stored and processed? | The vendor's stated regions, per the [Data Residency Policy](DATA_RESIDENCY.md) |
| Retention and deletion | How long is data kept, and can it be deleted? | Documented retention and a deletion mechanism |
| Compliance | Does it meet the obligations the project owes users? | Terms, DPAs, and certifications as relevant |
| Licence and terms | What are the terms of use, and can the project comply? | The licence, checked against `license-allowlist.json` and `license-exceptions.json` |
| Exit | How would the project replace it, and how long would that take? | A migration estimate and the data format involved |
| Track record | Has it had an outage, breach, or controversy? | Public record, and the project's own incident history |
| Maintenance | Is it maintained, and how will it be kept current? | Release activity and the project's update path |

Rules that apply to the assessment:

- **Necessity is assessed first.** A vendor that cannot justify its existence is not
  assessed on how safe it is, because not using it is the safest option available.
- **A missing answer is a finding.** A vendor that will not answer a security question
  is treated as having failed it, not as having passed by silence.
- **Self-declared certifications are a starting point.** A certificate is dated and
  scoped; the project's assessment is of the service it actually uses.
- **A new dependency is a vendor decision**, not a package-manager decision. Installing
  something is subject to this policy, as the
  [Bot and Automation Governance Policy](BOT_AUTOMATION.md) describes for automated
  updates.
- **Assessment happens before reliance, not after an incident.** A vendor handling data
  before its assessment is complete is a finding with a deadline, not a fait accompli.

## Risk Tiers

Vendors are tiered by the consequence of their failure or compromise, and the tier sets
what the project owes each one.

| Tier | Applies to | Requirements |
| --- | --- | --- |
| **Tier 1 — Critical** | Vendors handling authentication, personal data including children's data, payment, release signing, or the build chain | Security questionnaire answered, DPA in place, incident notification terms, encryption verified, annual reassessment, named owner, and a tested exit path |
| **Tier 2 — Standard** | Vendors handling limited or pseudonymised data, or providing non-security-critical runtime functions | DPA or terms reviewed, privacy and retention position documented, subprocessor list recorded, semiannual reassessment, named owner |
| **Tier 3 — Low** | Build-time and developer-experience tools that never see production user data | Licence and terms reviewed, recorded in the [Dependency Register](../registers/DEPENDENCY_REGISTER.md), annual reassessment on use rather than on schedule |
| **Tier 4 — Rejected** | Vendors that fail a Tier 1 or 2 criterion and will not remediate | Not used. The dependency is removed, or the function is rebuilt, or the feature is withdrawn |

Rules that apply across tiers:

- **The tier is set by consequence, not by how much the project likes the vendor.** A
  widely used vendor handling children's data is Tier 1.
- **A tier can move up.** A vendor is promoted immediately when it starts handling data
  it did not handle before, or when it is acquired by a party with a different risk
  profile.
- **A Tier 4 vendor is not a vendor the project keeps and dislikes.** It is either
  removed or accepted through an exception with an expiry.
- **A vendor used only in CI that receives a repository token** is Tier 1, because that
  token can write to the default branch. Build-time access to source is not a low risk.

## Review Cadence

| Vendor tier | Review cadence | Reviewer | Output |
| --- | --- | --- | --- |
| Tier 1 | **Every 12 months**, and on any material change | [Security Officer](../roles/SECURITY_OFFICER.md) | Re-approved, escalated with a remediation plan, or replaced |
| Tier 2 | **Every 12 months** | Maintainer owning the integration | Confirmed current, or re-tiered |
| Tier 3 | Every 12 months, or on use | Maintainer owning the integration | Licence and terms reconfirmed |
| Tier 4 | On rejection | [Security Officer](../roles/SECURITY_OFFICER.md) | Decision recorded, removal tracked |

Reviews are also triggered, regardless of the calendar:

- a vendor announces a breach, an outage, or a change in ownership;
- a vendor changes its subprocessors, regions, or retention period, which follows the
  notice process in the
  [Subprocessor Register](../registers/SUBPROCESSOR_REGISTER.md#change-notification-process);
- an advisory affects a vendor's product, handled under the
  [Vulnerability Remediation SLA Policy](VULN_SLA.md);
- a vendor is acquired, changes its terms materially, or discontinues a product the
  project depends on; or
- the project changes what it sends a vendor.

A review that finds drift updates the [Data Processing Register](../registers/DATA_PROCESSING_REGISTER.md)
and the [Subprocessor Register](../registers/SUBPROCESSOR_REGISTER.md) in the same change,
so that an assessment never disagrees with the registers.

## Ongoing Monitoring

Between reviews, the project watches for signals rather than waiting:

- **Dependency advisories**, from `audit.yml` on push and on the weekly schedule, routed
  through the [Vulnerability Remediation SLA Policy](VULN_SLA.md).
- **Licence changes** in a new version, which can move a dependency between categories
  without any code change; checked by `license-audit.js` and the
  [Dependency Register](../registers/DEPENDENCY_REGISTER.md).
- **Vendor status and incident pages**, checked at Tier 1 review rather than
  continuously, since no automated monitor is available for every vendor.
- **New subprocessor announcements**, which are a contractual notification rather than
  something to discover.

## Exit Planning

Every Tier 1 vendor has a written exit position, so that a vendor failure is a managed
event rather than an outage:

- **What would be replaced**, and with what alternative identified during vetting.
- **How long a migration would take**, stated as a rough range so it can be judged
  against the vendor's notification period.
- **Where the data lives and in what format**, so it can be exported before switching.
- **What happens to the data on exit**, including deletion by the vendor, confirmed
  under the [Right-to-Erasure Policy](DATA_DELETION.md) and the
  [DPA Template](../compliance/DPA_TEMPLATE.md).

A Tier 1 vendor with no identified alternative is a finding at review, not an accepted
state.

## Enforcement Status

- Automated signals exist and are real: `audit.yml` fails on high and critical
  dependency advisories on push and weekly, and `license-audit.js` checks licences
  against the allowlist.
- Vetting, tiering, and the review cadence are human activities. No system tracks a
  vendor's assessment date or prompts when one lapses, so the calendar is the control.
- The registers are the evidence base; their value depends on being updated when a
  vendor changes.

## Exceptions

A vendor used without a completed assessment, or above its tier, is recorded in the
[Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md) with the reason, the
compensating control, an approver holding the authority to grant it, and an expiry no
more than 12 months ahead. A vendor processing children's data without the
[Children's Data Policy](../compliance/CHILDRENS_DATA.md) conditions met cannot be
excepted under this policy.

## Ownership and Review

The [Security Officer](../roles/SECURITY_OFFICER.md) owns this policy. It is reviewed
when a new vendor category appears, when the tiering criteria change, and at least once
every 12 months. Changes are proposed in a pull request that touches only the
`Governance/` folder.

## Related Documentation

- [Subprocessor Register](../registers/SUBPROCESSOR_REGISTER.md) and
  [DPA Template](../compliance/DPA_TEMPLATE.md)
- [Data Processing Register](../registers/DATA_PROCESSING_REGISTER.md)
- [Dependency Register](../registers/DEPENDENCY_REGISTER.md) and
  [Risk Register](../registers/RISK_REGISTER.md)
- [Data Residency Policy](DATA_RESIDENCY.md) and
  [Record Retention and Legal Hold Policy](RECORD_RETENTION.md)
- [Vulnerability Remediation SLA Policy](VULN_SLA.md) and
  [Secret Rotation Policy](SECRET_ROTATION.md)
- [Children's Data Policy](../compliance/CHILDRENS_DATA.md)
- [Security Policy](../SECURITY_POLICY.md) and
  [Security Officer role](../roles/SECURITY_OFFICER.md)
- [Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md)
