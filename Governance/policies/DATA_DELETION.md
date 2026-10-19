# Right-to-Erasure Policy

## Purpose

This policy defines how a request to delete a person's data is received, verified,
executed, and confirmed, and what happens to data that must be kept instead. A deletion
promise the project cannot evidence is worse than no promise, so this document covers
the verification step as carefully as the deletion step.

## Scope

This policy covers any request to erase personal data held by TeachLink Mobile or by a
processor acting on its behalf, whatever the caller calls it — delete my account, remove
my data, right to erasure, right to be forgotten, or a school asking for a student's
records to be removed.

It covers:

- data in the primary application database, its replicas, and its backups;
- data held by subprocessors, per the
  [Subprocessor Register](../registers/SUBPROCESSOR_REGISTER.md);
- data cached on a user's device, which is under the user's own control; and
- data that cannot be deleted, and why.

It does not define how long data is otherwise kept, which is the
[Record Retention and Legal Hold Policy](RECORD_RETENTION.md); what is collected, which
is the [Privacy Manifest Policy](../domains/PRIVACY_MANIFEST.md); or where data is
stored, which is the [Data Residency Policy](DATA_RESIDENCY.md).

## How a Request Is Received

| Requester | Accepted through | Identity verification |
| --- | --- | --- |
| The account holder | The in-app deletion flow, or a written request to the address in the privacy policy | Authenticated session, or a response to a challenge sent to the registered contact address |
| A parent or guardian acting for a child | The written request channel, per the [Children's Data Policy](../compliance/CHILDRENS_DATA.md) | Proof of guardianship proportionate to the risk, which may mean direct confirmation with the school |
| An authorised school administrator | The written request channel, naming the learner | Confirmation with the school through an already-established contact, not through the requester alone |
| A regulator or supervisory authority | The written request channel | Verification of authority before any disclosure |

Rules that apply to every request:

- **A deletion request is never handled through a public issue, a review comment, or a
  public thread.** A request containing personal data is redirected to a private channel
  before anything else happens, and anything already posted publicly is treated as a
  disclosure.
- **Identity is verified before anything is deleted.** Deleting on an unverified request
  is a denial-of-service attack on a real account, and it is harder to detect than
  unauthorised access. Where verification is not possible, the project does not delete
  and says why.
- **No account identifier is confirmed to an unverified caller.** The response to an
  unverified request says only that the request cannot be processed without
  verification, not whether the account exists.
- **A verbal or informal request is honoured as a request.** It is not dismissed because
  it did not arrive on the right form; the project helps the requester complete the
  channel and records when the clock started.

## Timelines

The clock starts when a request is received, not when it is fully verified, so that a
slow verification cannot be used to extend the deadline indefinitely.

| Step | Target |
| --- | --- |
| Acknowledge receipt | **5 business days** |
| Verify identity and scope | **10 business days** |
| Execute deletion across the project and its subprocessors | **20 business days** |
| Confirm completion in writing | **30 business days** from receipt |

Rules that qualify the timelines:

- **A partial or staged deletion is still a deletion.** Data that cannot be removed
  immediately, such as a backup restored in the ordinary course, is scheduled for
  deletion and the schedule is given in the confirmation.
- **Where the law gives a shorter deadline, the shorter deadline applies.** The timeline
  above is the project's own commitment and never a way to exceed a statutory period.
- **A request that is refused is answered within the same 30 days**, with the reason and
  the route to challenge it. A silent refusal is not an answer.
- **A request that is complex is not automatically slower.** Where scope is unclear, the
  project asks one clarifying question and keeps working; it does not treat the question
  as a reset of the timeline.
- **Repeat requests do not restart the clock.** They are matched to the open request.

## Execution

Deletion is executed in a defined order so that partial completion is detectable.

1. **Primary store.** Personal data in the primary application database is deleted or
   anonymised, following the retention position for each field in the
   [Data Processing Register](../registers/DATA_PROCESSING_REGISTER.md).
2. **Related and derived data.** Search indexes, caches, analytics properties, support
   records, and exports are included. Data derived from the person is deleted or
   irreversibly anonymised, since pseudonymised data that can be re-identified is still
   personal data.
3. **Subprocessors.** Every processor holding the data is instructed to delete it within
   a period no longer than the project's own, and each one is confirmed. A processor
   that cannot comply within the window is escalated before the deadline, not after it.
   The [DPA Template](../compliance/DPA_TEMPLATE.md) requires this obligation.
4. **Backups and replicas.** Backups are deleted on the documented rotation, or are
   allowed to age out, and the project states which. Backup media is not rewritten to
   erase one person, because that damages the restore guarantee for everyone; the
   position is stated plainly to the requester rather than claimed as complete deletion.
5. **Device data.** Data held only on the user's device — the offline cache, local
   storage, and tokens — is deleted by the in-app flow or by uninstalling the app. The
   project states this plainly rather than implying it performed the deletion.
6. **Records that must be kept.** Data retained under a legal obligation is not deleted,
   is isolated so it cannot be used for any other purpose, and the requester is told
   what was retained and why. Retention follows the
   [Record Retention and Legal Hold Policy](RECORD_RETENTION.md).

## Verification

Deletion is verified before the project says it is done. Confirmation is a claim, and an
unverified claim is a liability waiting for an audit.

| Verification | How |
| --- | --- |
| The deletion ran | A dated deletion record exists, naming the request, the scope, the systems, and the timestamp |
| The data is gone | The deleted records are queried after deletion and return nothing, for each store in scope |
| Subprocessors confirmed | Each processor's written confirmation is retained with the record, and a non-responder is recorded as an exception rather than assumed compliant |
| Derived data is handled | Anonymisation, where used instead of deletion, is verified as irreversible by re-identification testing, not assumed from the absence of a key column |
| No copy survives in exports | Exports, backups, and caches are checked against the same list of locations the deletion covered |
| The confirmation is accurate | The written confirmation to the requester lists exactly what was deleted, what was retained and why, and what is scheduled for a later date |

A deletion is not confirmed complete when any part is unknown. Where a subsystem's state
cannot be verified, that fact is stated in the confirmation rather than omitted, and it
becomes a tracked follow-up.

## Children and Schools

Requests involving a child follow the
[Children's Data Policy](../compliance/CHILDRENS_DATA.md) in addition to this policy:

- the parent's or guardian's consent governs the account, and a child's own request is
  handled with the guardian's involvement unless the law says otherwise;
- school-held records are deleted at the school's instruction, since the school is the
  controller for the educational record; and
- the [Age Thresholds](../compliance/CHILDRENS_DATA.md#age-thresholds) in that policy
  determine who may request deletion for a given account.

## Refusal and Partial Refusal

A request may be refused or partially refused only for a stated legal reason — for
example, an obligation to retain financial records, or a legal claim. Refusal requires:

- the specific reason, not a generic reference to policy;
- the data actually retained, and the period it is kept;
- confirmation that the retained data is isolated and used only for the reason given;
- the route to challenge the refusal, through the
  [Conflict Resolution Process](../processes/CONFLICT_RESOLUTION.md); and
- a review date, because a refusal is reassessed rather than standing forever.

Refusing everything on a request is treated as a failure, not a preference. Where only
part of the scope is retained, the rest is deleted and the confirmation says which part
and why.

## Interaction with Retention and Backups

Deletion requests do not override a legal hold. Where a hold is in place, the project
tells the requester that the data is being retained under a legal obligation, states the
scope of the hold, and does not delete the held material. The hold itself follows the
[Record Retention and Legal Hold Policy](RECORD_RETENTION.md); erasure resumes when the
hold is released.

## Enforcement Status

- The project operates no backend of its own, so these controls are exercised through
  the providers recorded in the [Data Processing Register](../registers/DATA_PROCESSING_REGISTER.md)
  and enforced through the [DPA Template](../compliance/DPA_TEMPLATE.md).
- No automated erasure pipeline or verification harness exists in this repository. The
  verification table above is the specification a backend implementation must satisfy.
- The privacy policy must state the erasure channel and timeline; the
  [Privacy Policy Governance](../compliance/PRIVACY_POLICY_GOVERNANCE.md) owns that
  document and is updated when this policy changes.

## Exceptions

A request that cannot be completed within the timeline, or a scope that cannot be
verified, is recorded in the
[Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md) with the reason,
the compensating control, an approver holding the authority to grant it, and an expiry
no more than 12 months ahead. The requester is still answered within the timeline; an
exception never removes the obligation to respond.

## Ownership and Review

The [Security Officer](../roles/SECURITY_OFFICER.md) owns this policy, with the
[Release Manager](../roles/RELEASE_MANAGER.md) accountable for subprocessor deletion
confirmations. It is reviewed at least every 12 months, and after any failure to honour
a request. Changes are proposed in a pull request that touches only the `Governance/`
folder.

## Related Documentation

- [Record Retention and Legal Hold Policy](RECORD_RETENTION.md): how long data is kept
  and what a hold suspends
- [Consent Management Policy](CONSENT_MANAGEMENT.md): withdrawing consent
- [Data Processing Register](../registers/DATA_PROCESSING_REGISTER.md): what is
  processed and on what basis
- [Subprocessor Register](../registers/SUBPROCESSOR_REGISTER.md) and
  [DPA Template](../compliance/DPA_TEMPLATE.md): deletion obligations on processors
- [Children's Data Policy](../compliance/CHILDRENS_DATA.md): requests involving children
- [Privacy Manifest Policy](../domains/PRIVACY_MANIFEST.md) and
  [Privacy Policy Governance](../compliance/PRIVACY_POLICY_GOVERNANCE.md)
- [Security Policy](../SECURITY_POLICY.md): reporting a suspected breach instead
- [Conflict Resolution Process](../processes/CONFLICT_RESOLUTION.md): challenging a
  refusal
- [Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md)
