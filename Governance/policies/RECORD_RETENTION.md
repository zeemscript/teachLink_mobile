# Record Retention and Legal Hold Policy

## Purpose

This policy defines how long TeachLink Mobile keeps each category of data, what
triggers its deletion, and how a legal hold suspends deletion for the data it covers.
Keeping data longer than needed is a liability to the people it belongs to; deleting
data too eagerly can destroy records the project is obliged to retain. The resolution is
a written schedule with concrete triggers, not a judgement call per record.

## Scope

This policy covers every category of record the project holds, whether in the primary
database, in backups, or with a subprocessor. It covers:

- account and authentication records;
- student, teacher, and school records;
- lesson content and generated material;
- telemetry, analytics, and crash reports;
- audit and access records;
- financial and billing records;
- consent records, as evidence that consent was given;
- support and correspondence records;
- security and incident records; and
- governance records, which follow the retention conventions in this repository.

It does not cover how data is deleted on request, which is the
[Right-to-Erasure Policy](DATA_DELETION.md), or how long data is retained by a
subprocessor after the project instructs deletion, which is set by the
[DPA Template](../compliance/DPA_TEMPLATE.md).

## Retention Schedule

The schedule records the trigger as well as the period, because a period without a
trigger is unenforceable: a clock that starts when nothing happens is a clock that never
starts.

| Record category | Retention period | Trigger | Basis |
| --- | --- | --- | --- |
| Account profile data | Duration of the account, then 30 days | Account closure | Necessity of the service |
| Authentication tokens and sessions | Until expiry or logout, then immediately | Session end | Security |
| Student and school records | Duration of the enrolment, then the period below | Enrolment end | Legal or contractual record-keeping |
| Attendance and progress records | **7 years** from the end of the academic year | Academic year end | Statutory education record |
| Lesson content created by a teacher | Duration of the account, then 30 days | Account closure | User's own content |
| Generated and cached content | Duration of the account | Account closure | User's own content |
| Consent records | Duration of the consent plus **3 years** as evidence | Consent withdrawn or superseded | Evidence of lawful basis |
| Support correspondence | **2 years** from closure | Ticket closed | Service quality and dispute handling |
| Security and incident records | **6 years** from closure | Incident closed | Security investigation and accountability |
| Audit and access logs | **1 year**, rolling | Each event | Detection and accountability |
| Telemetry and analytics events | **13 months** | Each event | Product measurement, minimisation |
| Crash reports | **90 days** | Each report | Defect triage |
| Financial and billing records | **7 years** from the transaction | Transaction date | Statutory accounting requirement |
| Consent and privacy request correspondence | **3 years** | Request closed | Evidence of compliance |
| Governance documents and decision records | **Indefinite** | — | Public accountability; kept in the repository itself |
| Released build artifacts and provenance records | For the lifetime of the published release | Release | Reproducibility and incident response |

Rules that apply across the schedule:

- **A period is either concrete or it is not a period.** "As long as necessary" and
  "for a reasonable period" are not acceptable entries in this schedule.
- **The shortest defensible period is chosen.** Where a basis exists for a longer period
  it is named; where it does not, the period is the shorter one.
- **Deletion happens when the trigger fires**, on a defined schedule rather than
  opportunistically, so that a record is not kept indefinitely because nobody ran a job.
- **Backups follow the primary.** A backup is deleted on its own rotation or allowed to
  age out; the position is stated to anyone who asks under the
  [Right-to-Erasure Policy](DATA_DELETION.md), rather than claimed as immediate deletion.
- **Where the law requires longer, the law wins.** A statutory period overrides this
  schedule, and the entry is updated to say so rather than being quietly extended.

## Disposal Rules

Disposal is deletion, not hiding. Each category has a disposal method chosen for it.

| Method | Applies to | Notes |
| --- | --- | --- |
| Hard delete from the primary store | Profiles, sessions, tokens, message content | Immediate, with index and cache cleanup included |
| Irreversible anonymisation | Aggregate analytics where a record is needed for measurement | Verified irreversible, not merely stripped of an identifier |
| Expiry with a retention window | Telemetry, crash reports, support records | Deleted on the stated cycle |
| Rotate and age out | Backups | Deleted on the backup cycle; not rewritten for one record |
| Retain in full | Statutory records such as attendance and financial entries | Isolated, access-restricted, and used only for the statutory purpose |
| Retain in the repository | Governance documents | Public by design |

Rules that apply to disposal:

- **Disposal is verified, not assumed.** A deletion job that has not been observed
  removing data is a failure, and is fixed or escalated.
- **Anonymisation is tested.** Data that could be re-identified by joining on another
  table is not anonymised. Verification is part of the disposal, not a follow-up.
- **Disposal covers every copy.** Indexes, caches, exports, and subprocessor copies are
  included in the category, or the category entry says which are excluded and why.
- **Retention beyond the period requires a basis.** Extending a period is a change to
  this schedule, made through the normal governance process, not an operational choice.

## Legal Hold

A legal hold suspends deletion for the records it covers, whatever the schedule says.

1. **A hold is issued by a named role.** Only the [Security Officer](../roles/SECURITY_OFFICER.md)
   or a maintainer designated in the [Decision Rights Matrix](../matrices/DECISION_RIGHTS_MATRIX.md)
   may issue one, and every hold names the person who issued it.
2. **Scope is explicit.** The hold states the records covered, the matter it relates to,
   the date it was issued, and who to ask about its release. A hold with no scope is
   invalid.
3. **Effect is immediate.** On issue, scheduled deletion for the covered records is
   suspended, including automated jobs and backup rotation for that data.
4. **Covered data is isolated.** Held records are separated from ordinary processing so
   they are not used for any other purpose, and access is restricted to those who need
   it.
5. **People affected are told where they have a right to know.** A requester whose data
   is held is told that it is being retained and why, per the
   [Right-to-Erasure Policy](DATA_DELETION.md), unless law forbids disclosure.
6. **Review.** The hold is reviewed every 90 days and released when the matter closes.
   A hold that is never released is a defect, not a permanent condition.
7. **Record.** The hold, its scope, its issue and release dates, and the approver are
   recorded. Holds are never silently dropped.

A hold does not authorise new processing of the held data. It preserves what already
exists.

## Review of the Schedule

- The schedule is reviewed **every 12 months** by the
  [Security Officer](../roles/SECURITY_OFFICER.md) with the maintainers.
- It is reviewed immediately when a legal requirement changes, when a new category of
  data is introduced, when a subprocessor changes its retention, or when a
  [Right-to-Erasure Policy](DATA_DELETION.md) request cannot be honoured because of a
  retention entry.
- The review checks that each period is still the shortest defensible one. Schedules
  drift longer because nobody reduces a period, so reducing one is part of the review.
- Every period is checked against the
  [Data Processing Register](../registers/DATA_PROCESSING_REGISTER.md), and the two are
  corrected together.

## Enforcement Status

- The project operates no backend, so deletion is executed through the providers
  recorded in the [Data Processing Register](../registers/DATA_PROCESSING_REGISTER.md),
  with obligations set by the [DPA Template](../compliance/DPA_TEMPLATE.md).
- The repository contains no deletion job, and none of the retention periods in this
  schedule is machine-enforced. This document is the specification an implementation
  must satisfy.
- Where telemetry is in the app's own control, consent gates what is collected at all,
  which shortens the problem; see the
  [Mobile Telemetry Governance](../domains/MOBILE_TELEMETRY.md) and the
  [Consent Management Policy](CONSENT_MANAGEMENT.md).

## Exceptions

A category retained beyond its period, or a hold issued without the process above, is
recorded in the
[Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md) with the reason,
the compensating control, an approver holding the authority to grant it, and an expiry
no more than 12 months ahead. Where data concerns a child, the
[Children's Data Policy](../compliance/CHILDRENS_DATA.md) applies and minimisation is
required even where a longer period would otherwise be defensible.

## Ownership and Review

The [Security Officer](../roles/SECURITY_OFFICER.md) owns this policy, with the
[Release Manager](../roles/RELEASE_MANAGER.md) accountable for subprocessor retention.
It is reviewed every 12 months and after any legal change. Changes are proposed in a
pull request that touches only the `Governance/` folder.

## Related Documentation

- [Right-to-Erasure Policy](DATA_DELETION.md): deletion on request and its interaction
  with a hold
- [Consent Management Policy](CONSENT_MANAGEMENT.md): retaining consent as evidence
- [Data Processing Register](../registers/DATA_PROCESSING_REGISTER.md): what is
  processed, on what basis, and for how long
- [Subprocessor Register](../registers/SUBPROCESSOR_REGISTER.md) and
  [DPA Template](../compliance/DPA_TEMPLATE.md): retention on processors
- [Privacy Manifest Policy](../domains/PRIVACY_MANIFEST.md) and
  [Mobile Telemetry Governance](../domains/MOBILE_TELEMETRY.md)
- [Children's Data Policy](../compliance/CHILDRENS_DATA.md): minimisation for data
  about children
- [Incident Log](../registers/INCIDENT_LOG.md) and
  [Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md)
- [Security Officer role](../roles/SECURITY_OFFICER.md)
