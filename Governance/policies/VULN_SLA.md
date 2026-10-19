# Vulnerability Remediation SLA Policy

## Purpose

This policy sets a remediation deadline for every known vulnerability that affects
TeachLink Mobile, measured from the moment the project becomes aware of it, and defines
what happens when a deadline is missed. Acknowledging a report quickly is of little
value if nothing is fixed for a year; this document is about the fix, and about being
honest when the fix will be late.

## Scope

This policy covers remediation of vulnerabilities that are known to the project. It
begins when the project becomes aware of a vulnerability through any channel — a
private report, an automated advisory, a dependency alert, a store notice, a public
disclosure, or a maintainer's own finding — and ends when users are no longer exposed,
either because a fix has shipped or because the exposure is formally closed.

It covers vulnerabilities in:

- TeachLink Mobile's own application code;
- dependencies in the [Dependency Register](../registers/DEPENDENCY_REGISTER.md),
  including transitive ones reported by `audit.yml`;
- the build, release, and update-delivery chain, where a compromise of signing material
  is treated as Critical per the
  [Security Severity Rubric](../SECURITY_SEVERITY_RUBRIC.md);
- the mobile platform, such as an OS or SDK flaw affecting the app; and
- third-party services the app depends on, in coordination with the provider.

It does not govern how a vulnerability is reported. That is the
[Security Policy](../SECURITY_POLICY.md) and the
[Vulnerability Disclosure Process](../processes/VULN_DISCLOSURE.md).

## Remediation Targets by Severity

Severity is assigned using the
[Security Severity Rubric](../SECURITY_SEVERITY_RUBRIC.md#severity-levels), so that
the same level means the same thing everywhere. A project-specific adjustment is
permissible where the rubric does not fit, for example when a library flaw has no
reachable call path in the app; the adjustment and its reasoning are recorded with the
finding rather than applied silently.

| Severity | Fix or mitigation deployed | Update available to users | Notes |
| --- | --- | --- | --- |
| **Critical** | **24 hours** from awareness | 7 calendar days | Remote exploitation without interaction, account takeover, data loss, or compromise of the release-signing chain |
| **High** | **7 calendar days** from awareness | 14 calendar days | Limited-interaction or limited-privilege exploitation of user data, payment, or authentication |
| **Medium** | **30 calendar days** from awareness | Next regular release | Meaningful interaction or unusual conditions required; targeted exposure or denial of service |
| **Low** | **90 calendar days** from awareness | Next regular release | Limited impact, self-inflicted harm, or best-practice hardening |
| **Informational** | No fixed deadline | Not applicable | Recorded, and acted on only if it becomes part of a larger fix |

Two rules qualify every row:

- **Mitigation counts only if it removes the exposure.** A change that reduces severity
  without removing the vulnerable path is not a mitigation. A workaround that blocks
  the exploitation path, such as disabling the affected feature behind a flag, counts;
  a comment, a note, or a monitoring rule does not.
- **A fix with no release path is not a fix.** The deadline that matters to a user is
  the one in the "available to users" column. Where app-store review gates a release,
  the target includes that lead time, and a delay caused by store review is recorded as
  such rather than counted against the team.

The rubric's acknowledgement targets — 4 business hours for Critical, 1 business day
for High — continue to apply unchanged and are measured separately.

## Measurement

Every vulnerability is tracked as an issue so that the clock is visible and auditable.

### What starts the clock

The clock starts at the **time of awareness**, which is the earliest of:

- the moment a private report reaches the project, recorded by the
  [Security Officer](../roles/SECURITY_OFFICER.md);
- the moment an automated advisory matching an affected dependency or image is published;
- the moment a store or platform vendor notifies the project; or
- the moment a maintainer identifies the issue in their own work.

Awareness time, not publication time, is the reference. A vulnerability published
yesterday and discovered today starts today, and the days it was public before
discovery count against the provider only if the provider was in a position to notify
the project.

### What stops the clock

The clock stops when the exposure ends for users:

- a release containing the fix has **shipped and is available**; or
- the vulnerability is formally closed because it is not exploitable in this app, which
  requires a documented reachability argument, not an absence of evidence of exploit.

A fix merged to `main` but not yet released does not stop the clock. The gap between
merge and release is the project's responsibility and is planned for, not treated as
waiting time.

### What is recorded

| Field | Content |
| --- | --- |
| Awareness timestamp | With time zone, and the source that triggered it |
| Severity and rationale | The level, and why it matches the rubric |
| Affected versions | Which released app versions and which dependency versions are affected |
| Exposure scope | Which users and which data, per the [Data Processing Register](../registers/DATA_PROCESSING_REGISTER.md) |
| Owner | The maintainer accountable for the fix |
| Milestones | Reproduce, fix merged, fix released, advisory published |
| Actual times | The real timestamps for each milestone, used to compute the variance |

These fields match the issue templates and the
[Incident Log](../registers/INCIDENT_LOG.md), so a vulnerability that becomes an
incident is not re-described from scratch.

### Tracking the targets

- Open vulnerabilities are listed with the days remaining against their targets at the
  monthly security review, which is the security counterpart of the reporting in the
  [Code Review Service Level Agreement](REVIEW_SLA.md).
- A vulnerability approaching 80 percent of its window is flagged to the Security
  Officer and the Release Manager.
- The measurement method is deliberately simple and stated here so that it cannot be
  quietly adjusted per finding: start at awareness, stop at release, count calendar
  days, and record both.

## Escalation When a Target Is Missed

A missed target is a signal to change the plan, not to relabel the severity.

1. **Declare it.** Within one business day of the target being missed, the owner marks
   the issue as missed and states why: not started, in progress, blocked, awaiting
   review, awaiting store review, or awaiting a third party.
2. **Escalate immediately** for Critical and High. The [Escalation Path](../processes/ESCALATION_PATH.md)
   applies, and the Release Manager decides whether the fix ships as an expedited
   release outside the normal cadence. A security fix may not wait for a milestone that
   is weeks away.
3. **Tell the users when the exposure is user-facing.** If a released version is
   affected and no fix is available, the disclosure decision follows the coordinated
   process in the
   [Security Severity Rubric](../SECURITY_SEVERITY_RUBRIC.md#coordinated-disclosure),
   including notifying store listings if that is what reaches users.
4. **Record the cause.** A missed target without a stated cause is treated as an
   incomplete record and is raised at the next security review.
5. **Fix the system, not just the instance.** Where the cause is structural — no
   automated dependency alert, a test that does not cover the affected path, a single
   maintainer who alone understands the area — a follow-up task is created to remove
   that dependency, so the next occurrence is less likely to be late.
6. **Do not repeat silently.** A second missed target for the same vulnerability, or a
   pattern of misses in one severity band, is escalated to the maintainers as a
   governance item.

Missing a target is not itself a fault, provided it is declared, escalated, and
explained. Hiding it, or silently extending the deadline, is.

## Dependencies and Third Parties

- An automated advisory with no fix available is remediated by mitigation: the affected
  dependency is removed, replaced, or disabled, and the exposure is recorded with the
  provider's response time.
- A fix is merged within the target window even when the provider has not shipped one,
  by pinning to a safe version, backporting the fix, or removing the feature path.
- A third-party service vulnerability that the project cannot mitigate is recorded as
  accepted risk in the [Risk Register](../registers/RISK_REGISTER.md) with the
  compensating control, and is reported to users if their data is affected.
- Renovate and `audit.yml` handle routine dependency updates; a high or critical
  advisory bypasses the normal update cadence and is treated as a Critical or High
  finding by severity, not by convenience.

## Exceptions

A remediation target that cannot be met for a documented reason — a platform vendor
with no fix and no workaround, a store review outside the project's control, a
dependency that cannot be removed without breaking the app — is recorded in the
[Security Exceptions Register](../registers/SECURITY_EXCEPTIONS_REGISTER.md), with the
compensating control and an expiry no more than 12 months ahead. An exception is not
available for "we did not get to it".

## Enforcement Status

- `audit.yml` runs on push and on a weekly schedule and fails on high and critical
  dependency advisories, which is the automated half of dependency remediation. Its
  checks are the repository's real defence for the dependency case.
- Advisory matching, exposure analysis, and user notification are manual. Nothing
  currently pages anyone when an advisory appears or when a target is missed, so
  measurement depends on a maintainer reading the weekly audit run.
- Automation for advisory matching and for target tracking is a reasonable follow-up and
  would be introduced under the
  [Bot and Automation Governance Policy](BOT_AUTOMATION.md).

## Ownership and Review

The Security Officer owns this policy. It is reviewed after any missed Critical or High
target, when the severity rubric changes, when a new automated advisory source is
added, and at least once every 12 months. Changes are proposed in a pull request that
touches only the `Governance/` folder.

## Related Documentation

- [Security Severity Rubric](../SECURITY_SEVERITY_RUBRIC.md): severity levels and
  acknowledgement SLAs
- [Security Policy](../SECURITY_POLICY.md): how to report a vulnerability
- [Vulnerability Disclosure Process](../processes/VULN_DISCLOSURE.md): coordination with
  reporters
- [Escalation Path](../processes/ESCALATION_PATH.md): what happens when a target is
  missed
- [App Signing Key Policy](../domains/APP_SIGNING_KEYS.md): compromise of the
  release-signing chain
- [Dependency Register](../registers/DEPENDENCY_REGISTER.md) and
  [Risk Register](../registers/RISK_REGISTER.md)
- [Incident Log](../registers/INCIDENT_LOG.md) and
  [Security Exceptions Register](../registers/SECURITY_EXCEPTIONS_REGISTER.md)
- [Security Officer role](../roles/SECURITY_OFFICER.md) and
  [Release Manager role](../roles/RELEASE_MANAGER.md)
