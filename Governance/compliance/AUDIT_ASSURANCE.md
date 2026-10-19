# Audit and Assurance Policy

## Purpose

This policy defines how the project's controls are audited, how often, what evidence is
retained, and how findings are remediated. Governance documents that nobody checks are
aspirations. This policy makes assurance an activity with a schedule, evidence, and an
obligation to act on what it finds.

## Scope

This policy covers assurance activity over the project's governance and operational
controls, including:

- governance documents: whether each policy still reflects how the project works, and
  whether the registers are being maintained;
- access and identity: the [Access Register](../registers/ACCESS_REGISTER.md) and the
  [Access Review Policy](../policies/ACCESS_REVIEW.md);
- security controls: the [Security Policy](../SECURITY_POLICY.md), the
  [Secret Rotation Policy](../policies/SECRET_ROTATION.md), and the
  [Key Management Policy](../policies/KEY_MANAGEMENT.md);
- data protection controls: the [Record Retention and Legal Hold Policy](../policies/RECORD_RETENTION.md),
  the [Right-to-Erasure Policy](../policies/DATA_DELETION.md), and the
  [Consent Management Policy](../policies/CONSENT_MANAGEMENT.md);
- supply chain: the [Dependency Register](../registers/DEPENDENCY_REGISTER.md),
  licence compliance, and the
  [Vulnerability Remediation SLA Policy](../policies/VULN_SLA.md);
- release controls: the
  [EAS Build Governance](../domains/EAS_BUILD_GOVERNANCE.md) and the
  [App Signing Key Policy](../domains/APP_SIGNING_KEYS.md); and
- process controls: triage, review, escalation, and the community policies.

It does not govern an external audit commissioned on the organisation, nor a regulatory
inspection. Where an external audit is arranged, this policy's evidence set is what makes
that audit faster.

## Audit Scope and Cadence

| Assurance activity | Scope | Cadence | Owner |
| --- | --- | --- | --- |
| Governance review | Are policies current, registers maintained, decisions recorded | **Quarterly** | Maintainers |
| Access review | Privileged access, workflow secrets, service accounts | **Quarterly** | [Security Officer](../roles/SECURITY_OFFICER.md) |
| Security posture review | Incident history, vulnerability targets, secret and key hygiene | **Quarterly** | [Security Officer](../roles/SECURITY_OFFICER.md) |
| Data protection review | Processing register, retention, consent, subprocessor changes | **Every 12 months**, or quarterly where child data is involved | [Security Officer](../roles/SECURITY_OFFICER.md) |
| Supply chain review | Dependency register, advisories, licence allowlist | **Every 12 months** | Maintainer responsible for dependencies |
| Release control review | Signing custody, promotion path, store submissions | **Every 12 months** and before a major release | [Release Manager](../roles/RELEASE_MANAGER.md) |
| Continuous verification | What the CI workflows and `audit.yml` actually check | **Every 12 months** | Maintainer responsible for CI |
| Independent review | A second pair of eyes on a named area | **Every 24 months**, and on request | [Community Manager](../roles/COMMUNITY_MANAGER.md) arranges |

Rules that apply to the schedule:

- **A skipped review is recorded as skipped**, with the reason. A gap that is documented
  can be planned for; a gap that is invisible cannot.
- **A finding in one area expands the review of that area.** A failure found in a
  quarterly review is a reason to look at the neighbouring control, not only to fix the
  finding.
- **Child data raises the cadence.** Where a control protects data about children, the
  quarterly cadence applies, per the
  [Children's Data Policy](../compliance/CHILDRENS_DATA.md).

## Evidence Retention

Evidence is what makes a control verifiable later. An assurance activity that leaves no
evidence has not happened, however thoroughly it was performed.

| Evidence | Retention | Where it lives |
| --- | --- | --- |
| Review records and their outcomes | 3 years | The relevant register or the governance review record |
| Access review findings | 3 years | [Access Register](../registers/ACCESS_REGISTER.md) |
| Secret and key rotation records | 3 years | The rotation calendar and the relevant issue |
| Security incident records | 6 years | [Incident Log](../registers/INCIDENT_LOG.md) |
| Vulnerability remediation measurements | 3 years | The tracking issue, per the [Vulnerability Remediation SLA Policy](../policies/VULN_SLA.md) |
| Consent records and deletion confirmations | Duration of consent plus 3 years | The controlled consent store |
| External audit reports, where obtained | As the auditor requires | Private storage, summary published |
| CI run results and required-check outcomes | 90 days, per the provider's retention | CI provider |

Rules that apply to evidence:

- **Evidence records what was checked and what was found**, not only that a check passed.
  A list of "reviewed, no findings" is itself the evidence that the review happened.
- **Evidence never contains secrets or personal data.** A rotation record states that a
  key was rotated, not what the key was.
- **Evidence is retained even when the control failed.** A finding record is retained and
  linked to its remediation, which is how repeat findings are detected.
- **Evidence follows the retention schedule** in the
  [Record Retention and Legal Hold Policy](../policies/RECORD_RETENTION.md), and is
  subject to a legal hold when one is in place.

## Finding Remediation

A finding is only useful if it leads to a change. Every finding follows the same path.

| Severity | Definition | Remediation deadline |
| --- | --- | --- |
| **Critical** | A control that does not exist and should, exposing personal data or enabling unauthorised access | **7 calendar days** |
| **High** | A control that exists but is not operating, or a material gap in a documented control | **30 calendar days** |
| **Medium** | A control operating partially, or a documentation and practice mismatch | **90 calendar days** |
| **Low** | An improvement opportunity, or a register and practice divergence with low consequence | Next scheduled review |

The remediation process:

1. **Record the finding** with the control, the evidence, the severity and why, and the
   owner accountable for fixing it.
2. **Plan the fix** with a date. A finding with no date is a finding with no deadline.
3. **Remediate**, or record why it cannot be remediated as found.
4. **Verify the fix.** The same person who verified the control found the gap confirms
   the closure, where practicable. Self-certification of a High or Critical closure
   requires a second reviewer.
5. **Close with the evidence recorded**, linked to the original finding, so that a later
   reviewer can see what was wrong and what changed.
6. **Escalate on a missed deadline**, through the
   [Escalation Path](../processes/ESCALATION_PATH.md), rather than letting the date pass
   and the finding quietly ageing.

A finding that recurs after being closed is treated as a **systemic** finding: the
individual fix is secondary to why the same gap reappeared. Systemic findings go to the
maintainers as a governance item.

### Exceptions

A finding accepted rather than fixed is recorded in the
[Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md) with the reason, the
compensating control, an approver holding the authority to grant it, and an expiry no more
than 12 months ahead. A finding about a missing control is not improved by noting it, and
renewal is never automatic: a second approval is a signal to fix it.

## Continuous Verification

Some controls are verified automatically, and it is worth being clear about which, since a
policy that implies automation that does not exist is misleading.

| Control | Verified by | Frequency |
| --- | --- | --- |
| High and critical dependency vulnerabilities | `audit.yml` | On push and weekly |
| Disallowed dependency licences | `license-audit.js` and `audit.yml` | On push |
| Architecture boundaries | `dependency-cruiser` in `architecture.yml` | On pull request |
| Lint, format, typecheck, tests, OpenAPI validation | The `ci` job | On pull request |
| Bundle size and API performance budgets | `checkBundleSize.js`, `checkApiPerf.js` | On pull request |
| Build and release provenance | The release workflow and tagged releases | Per release |
| Branch protection rules | Maintainer confirmation in the repository settings | Every 12 months |

The controls verified only by human review — registers, rotation cadence, access
reviews, consent records, vendor assessments, exception expiry — are stated as such in
each policy's enforcement-status section. The Continuous Verification activity exists
precisely to confirm that this table still matches reality, because an inventory of
automation drifts as workflows change.

## Reporting

Each assurance activity produces a short record:

- the date, the reviewers, and the scope reviewed;
- the controls examined and the evidence used;
- findings raised with their severity and owner;
- the outcome for each, including "no change needed"; and
- anything that could not be verified, and why.

The record is published where it contains nothing sensitive. A summary of each quarterly
governance review is published to contributors so that the project is accountable for
what it claims; the full record, where it names individuals or includes security
detail, is kept private and its existence stated.

## Enforcement Status

- Everything in the Continuous Verification table is real and runs in this repository
  today. The human-review activities are not automated, and are tracked through the
  register and review records rather than a tool.
- Branch protection settings live outside the repository and are confirmed by a
  maintainer rather than read by CI.
- No external audit has been commissioned, and none is claimed. This document describes
  what the project does for itself, and says so plainly rather than implying assurance it
  does not have.

## Exceptions

Any deviation from the cadence or from the evidence requirement is recorded in the
[Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md) with an approver
holding the authority to grant it and an expiry no more than 12 months ahead. A review
that cannot be performed because of a staffing gap is an exception to record, not a
review to skip quietly.

## Ownership and Review

The [Security Officer](../roles/SECURITY_OFFICER.md) owns this policy, with the
[Release Manager](../roles/RELEASE_MANAGER.md) responsible for release-control assurance
and the [Community Manager](../roles/COMMUNITY_MANAGER.md) for publishing the summaries.
It is reviewed at least once every 12 months, and after any Critical or High finding.
Changes are proposed in a pull request that touches only the `Governance/` folder.

## Related Documentation

- [Children's Data Policy](../compliance/CHILDRENS_DATA.md): the raised cadence for
  child data
- [Access Register](../registers/ACCESS_REGISTER.md) and
  [Access Review Policy](../policies/ACCESS_REVIEW.md)
- [Incident Log](../registers/INCIDENT_LOG.md) and
  [Risk Register](../registers/RISK_REGISTER.md)
- [Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md) and
  [Security Exceptions Register](../registers/SECURITY_EXCEPTIONS_REGISTER.md)
- [Security Policy](../SECURITY_POLICY.md) and
  [Secret Rotation Policy](../policies/SECRET_ROTATION.md)
- [Record Retention and Legal Hold Policy](../policies/RECORD_RETENTION.md): evidence
  retention
- [Escalation Path](../processes/ESCALATION_PATH.md): missed remediation deadlines
- [CI/CD Governance Policy](../policies/CICD_GOVERNANCE.md): the pipelines whose checks
  are listed here
- [Security Officer role](../roles/SECURITY_OFFICER.md)
