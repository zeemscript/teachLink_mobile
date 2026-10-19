# Code Review Etiquette Guide

## Purpose

This guide sets the expectations for code review as a human interaction: the tone, what
authors can expect from reviewers and reviewers from authors, and how disagreement is
handled so that it produces better code rather than worse relationships. The project's
[Code Review Policy](../policies/REVIEW_POLICY.md) governs *what* is reviewed and who may approve;
this guide is about how the conversation is conducted.

## Scope

This guide applies to review of pull requests, issues, and proposed changes anywhere in
the project — including governance documents, which are reviewed with the same care as
code. It covers the conduct expected of both sides.

It does not set the technical bar, the approval counts, or the turnaround commitments.
Those are in the [Code Review Policy](../policies/REVIEW_POLICY.md), the
[PR Approval Requirements](../policies/APPROVAL_REQUIREMENTS.md), and the
[Code Review Service Level Agreement](../policies/REVIEW_SLA.md).

## Why This Matters

Code review is where most people first experience how the project treats them. A
contributor whose first review was patient and specific comes back. One that was a
string of terse corrections, or a public pile-on, does not. That is the whole reason
this document exists: review quality is a retention mechanism as much as a quality
mechanism.

## For Reviewers

### Tone

- **Write to the author, not about them.** "This allocates twice" rather than "you
  allocated twice" is not a pedantic rule; it makes a comment about the code and not a
  verdict on the person.
- **Assume competence and good faith.** The author may know something you do not, and may
  have a constraint you were not told about.
- **Say what you mean.** "This is unclear" is not feedback. Say which line and what is
  unclear about it.
- **Praise specifically when something is good.** Not as a cushion before criticism, but
  because a specific "this abstraction is exactly right, and here is why it will hold up"
  teaches as much as a correction.
- **No sarcasm, no jabs, no "nitpicking your lack of tests" as an opening.** Jokes that
  single someone out are not review.
- **Do not relitigate the project's decisions in a review thread.** If a design decision
  is wrong, that is a discussion in an issue or an RFC, not a series of one-line comments
  on someone's pull request.

### Substance

- **Distinguish what blocks from what does not.** Label blocking comments clearly, and
  separate required changes from preferences and questions. A reviewer who marks
  everything required teaches authors to ignore the labels.
- **Explain the why, not just the what.** "Use a constant here" is an instruction. "Use a
  constant here, because this value appears in three places and has already drifted
  between two of them" is review.
- **Be specific about correctness.** If you believe something is wrong, give the input
  that breaks it. "This will fail if X" is answerable; "this looks fragile" is not.
- **Offer the alternative you would accept.** A blocking comment without a path forward
  transfers the design work to the author.
- **Read the whole change before commenting on a part.** Reviewing the third hunk first
  produces comments that the first hunk already contradicts.
- **Approve what is good.** Withholding approval to reserve leverage is a misuse of the
  mechanism. If the change is ready, approve it.

### Scope

- **Review the change in front of you.** Unrelated problems are noted separately, as a
  new issue rather than as scope added to this pull request.
- **Check that the change does what it says**, including the tests and the documentation.
  A missing test or an unupdated policy is a legitimate review comment.
- **Review for security, correctness, and maintainability first.** Style nits come last
  and never block on their own; where the project has automated formatting and linting,
  a comment that a tool would have caught is a comment about the tool configuration, not
  the author.

## For Authors

- **Assume the reviewer is trying to help.** Where that is not what happened, the
  [Conflict Resolution Process](../processes/CONFLICT_RESOLUTION.md) exists, and using it
  is legitimate.
- **Explain context in the pull request.** Reviewers are frequently missing the constraint
  you worked under. A sentence about why saves three review cycles.
- **Respond to every comment**, even if only to say you disagree and why. An unanswered
  comment stalls the review and, more importantly, leaves the other person unsure.
- **Disagree in the thread, with a reason.** Most disagreements resolve once the
  underlying fact is on the table.
- **Take feedback on the code, not on the tone.** A blunt comment often contains a real
  point. Answer the point, and raise the tone separately if it needs raising.
- **Do not push new commits to a pull request already under review** without saying so.
  It forces reviewers to start again.
- **Say when you are done and what remains.** If you are still working, or you believe the
  change is not ready, say so — an unfinished draft is fine, an unfinished one presented
  as finished is not.

## Handling Disagreement

Most review disagreement is a missing fact rather than a conflict of values. The
sequence below is tried in order.

1. **Ask, once, for the underlying fact.** "What breaks if this is nil here?" is often
   answered with "nothing — good catch".
2. **State your position with a reason.** A position with a reason can be discussed; one
   without becomes a matter of taste.
3. **Bring in a third maintainer.** Where two people have both reasoned carefully and
   still disagree, a third reviewer decides, per the
   [Decision Rights Matrix](../matrices/DECISION_RIGHTS_MATRIX.md).
4. **Move it to the right venue.** A disagreement about a design belongs in an issue or an
   RFC under the [RFC Process](../processes/RFC_PROCESS.md), not in a pull request
   thread that blocks a merge.
5. **Escalate if it is not resolving.** Through the
   [Escalation Path](../processes/ESCALATION_PATH.md) or the
   [Conflict Resolution Process](../processes/CONFLICT_RESOLUTION.md).
6. **Code of conduct, separately.** Persistent hostility, personal criticism, or
   discrimination during review is a conduct matter, not a review disagreement, and goes
   to the [Code of Conduct Reporting](../COC_REPORTING.md) process. It is raised on its
   own terms and never used as leverage in a technical argument.

Rules that hold throughout:

- **No blocking over style.** A reviewer who will not approve over formatting when a tool
  handles formatting is overruling the project's automation.
- **The author can walk away.** Someone who feels review has become abusive can stop
  participating. That is a legitimate outcome, and the conduct is addressable regardless
  of whether the pull request ever merged.
- **Good faith is assumed, and so is a bad day.** Reviewers are volunteers with their own
  work. A terse comment is addressed, not escalated on its own.

## Governance and Documentation Reviews

Documentation and governance changes are reviewed on the same terms as code, with two
adjustments:

- **Factual claims are verified.** A policy document that asserts something untrue about
  the repository is a defect. Checkable statements are checked, and where a statement
  cannot be verified from the repository, the document says so.
- **Scope matters.** Governance changes stay inside `Governance/`, as the
  [Governance README](../README.md) requires, so that a documentation change cannot
  silently alter behaviour.
- **A policy that describes a control that does not exist is worse than no policy.** A
  reviewer should challenge a claim of enforcement that is not implemented, and the
  document should state plainly what is enforced today and what is not.

## Enforcement

Review etiquette is enforced by the
[Code of Conduct Enforcement](../COC_ENFORCEMENT.md), not by a separate mechanism.
Persistent or severe violations are handled under that policy and the
[Privilege Revocation Policy](../policies/REVOCATION.md), which is how review powers are removed when
someone uses them badly.

Review quality is also checked in the monthly governance review: the number of changes
requested, the number of pull requests that went quiet, and recurring comments that
suggest a missing convention or a missing automated check.

## Related Documentation

- [Code Review Policy](../policies/REVIEW_POLICY.md): what is reviewed, by whom, and how
- [PR Approval Requirements](../policies/APPROVAL_REQUIREMENTS.md): approval counts and required
  checks
- [Code Review Service Level Agreement](../policies/REVIEW_SLA.md): turnaround commitments
- [Conflict Resolution Process](../processes/CONFLICT_RESOLUTION.md) and
  [Escalation Path](../processes/ESCALATION_PATH.md)
- [Objection Handling Process](../processes/OBJECTION_HANDLING.md) and
  [RFC Process](../processes/RFC_PROCESS.md)
- [Code of Conduct Enforcement](../COC_ENFORCEMENT.md) and
  [Code of Conduct Reporting](../COC_REPORTING.md)
- [Decision Rights Matrix](../matrices/DECISION_RIGHTS_MATRIX.md)
- [Stale Pull Request Policy](../policies/STALE_PRS.md) and
  [Inclusive Language Guide](INCLUSIVE_LANGUAGE.md)
