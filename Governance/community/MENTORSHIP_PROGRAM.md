# Mentorship Program

## Purpose

This policy defines how mentors and mentees are matched, what each side can expect, and
how a mentorship runs to a defined end. TeachLink Mobile is a React Native project with
its own conventions, and the gap between "can write JavaScript" and "can contribute
comfortably here" is where new contributors are lost. Mentorship closes that gap
deliberately rather than by hoping review comments add up to an education.

## Scope

This policy covers the mentorship relationship: who may mentor, how matches are made,
what is expected of each party, how long a mentorship lasts, and how it ends.

It complements the mentor step already described in the
[Contributor Onboarding Process](../processes/ONBOARDING.md#mentor-program), which
introduces the program during the first weeks. That step sets the entry point; this
document is the program itself. It does not set role requirements, which are in the
[Role Promotion Criteria](../processes/PROMOTION_CRITERIA.md).

## Who Can Mentor

Mentoring is voluntary and never a condition of holding a role.

| Requirement | Detail |
| --- | --- |
| Baseline role | [Reviewer](../roles/REVIEWER.md) or above, so that a mentor can review the mentee's work and vouch for it |
| Opt-in | A maintainer chooses to mentor. Nobody is assigned because they are a maintainer |
| Stated capacity | A mentor states how many mentees they can take and for how long, so that availability is real rather than assumed |
| Area of expertise | Stated in terms of the areas they can actually help with: user interface, native platform, testing, documentation, accessibility, data protection, release engineering |
| Training | A mentor reads this policy and the project's expectations for review, per the [Code Review Policy](../policies/REVIEW_POLICY.md), before taking a mentee |

A mentor who cannot meet the time commitment says so and stops rather than letting a
mentee wait. Handing a mentorship on is expected and is not a failure.

## Matching

Matches are made on the mentee's needs rather than on availability.

1. **The mentee states their goals.** Area of interest, what they want to be able to do
   in three months, and what has made previous contributions hard.
2. **The need is classified.** Mentorship requests are one of: orientation and setup,
   a specific technical area, review confidence, or route to a first maintainer-level
   contribution.
3. **Mentors are matched by area and language.** A mentor in the relevant area is
   preferred. Where the mentee's primary language is not the project's working
   language, a mentor comfortable in the mentee's language is preferred, because the
   project serves contributors worldwide and a language barrier is an access barrier.
4. **Timezone is a tiebreaker, not the reason.** Matching on availability alone produces a
   mentor who is unavailable when the mentee needs help.
5. **The match is proposed, not imposed.** The Community Manager proposes a match and
   both people confirm before it starts. A poor match is resolved by rematching, not by
   the mentee declining to continue.
6. **The match is recorded.** Who was matched to whom, in which area, with the start
   date, so the program has a record rather than a memory.

## Expectations

### What a mentee can expect

- A named mentor who has agreed to the time commitment.
- A first response to a question within **two business days**, and a weekly synchronous
  conversation during the initial cycle.
- A first review of their opening pull requests with more explanation than the standard
  review a maintainer would give, per the
  [Code Review Policy](../policies/REVIEW_POLICY.md).
- An introduction to the people and channels relevant to their work.
- An honest answer about whether their work is good, including when it is not. A mentor
  who only encourages is not mentoring.
- Help understanding the project, not a replacement for reading the documentation. The
  mentor points at the source of truth rather than answering from memory.

### What a mentor can expect

- A mentee who turns up, and who says so when they cannot.
- Questions in good faith, including basic ones. A mentor who treats a basic question as
  a failing is wasting the relationship.
- A defined end. Mentorship is not an indefinite obligation.
- A route out: a rematch, a pause, or a clean ending, without having to justify it to
  the project.

### What neither side expects

- Unpaid production work. A mentorship does not entitle a mentee to assign work or to
  be handed tasks in exchange for the mentoring. This is a volunteer program; the
  [Contributor Agreement](../community/CONTRIBUTOR_AGREEMENT.md) is the basis on which
  contribution is offered.
- Authority. A mentee gains no permissions from being mentored, and a mentor grants
  none.
- A guaranteed outcome. Nobody is promised a maintainer role, a specific issue, or a
  place on a milestone team.

## Duration and Review

| Phase | Duration | Focus |
| --- | --- | --- |
| Initial cycle | **4 weeks** | Setup, orientation, first contributions, with the weekly commitment in the [onboarding process](../processes/ONBOARDING.md#mentor-program) |
| Extension | Up to **3 months** total | Deeper work in the mentee's chosen area, with fortnightly contact |
| Review point | At week 4, and at month 3 | Structured check-in as below |
| Wind-down | Last 2 weeks | The mentee runs the process alone, with the mentor available |

The structured check-in asks three questions, in writing, so that the answers are
visible later:

1. What can the mentee now do that they could not at the start?
2. What is still blocking them, and is it a mentoring gap or something else — such as an
   unclear requirement, an environment problem, or a language or accessibility barrier?
3. Does the match still make sense?

Where the answer to the second question is "something else", the mentoring is not the
right fix and is redirected: to the [Support Policy](../policies/SUPPORT.md), the
[Contributor Onboarding Process](../processes/ONBOARDING.md), or the
[Escalation Path](../processes/ESCALATION_PATH.md).

## Review and Outcomes

The program is reviewed by the [Community Manager](../roles/COMMUNITY_MANAGER.md) every
six months:

| Measure | How it is gathered |
| --- | --- |
| Mentee satisfaction | A short anonymous survey at the end of each cycle |
| First-pull-request experience | Whether the mentee's opening pull requests received an explanatory review rather than only corrections |
| Time to first merged contribution | Compared against the onboarding metrics already tracked monthly |
| Retention | Whether mentees remain active six months later, compared against contributors who were not mentored |
| Mentor load | Whether the stated capacity was realistic, and whether mentors are over-committed |
| Rematch rate | How often a match did not work, which is the clearest signal that matching needs attention |

The last point is the one most easily ignored: a high rematch rate is a defect in
matching, not a defect in the people who were matched.

## Recognition

Mentoring is credited under the
[Contributor Recognition and Credits Policy](../RECOGNITION.md), and a mentor may
include mentorship in a nomination for
[Emeritus](../roles/EMERITUS.md) or a maintainer role, since it is evidence of
investment in the project rather than only of code output. Recognition is not a
substitute for the time commitment.

## Enforcement Status

- The program is a human arrangement. Nothing automates matching, reminders, or
  check-ins.
- Meeting outcomes are measured from issue, pull request, and contributor records that
  already exist for the onboarding metrics; no new data collection is required, and none
  is gathered that would identify an individual to their disadvantage.
- The record of matches is maintained by the Community Manager.

## Related Documentation

- [Contributor Onboarding Process](../processes/ONBOARDING.md): where mentorship enters
  the onboarding path
- [Newcomer Welcome Guide](WELCOME_GUIDE.md): the steps before and after matching
- [First-Time Contributor Policy](../policies/FIRST_TIME_CONTRIBUTOR.md) and
  [Good-First-Issue Policy](../policies/GOOD_FIRST_ISSUE.md)
- [Code Review Policy](../policies/REVIEW_POLICY.md) and
  [Code Review Service Level Agreement](../policies/REVIEW_SLA.md)
- [Contributor Recognition and Credits Policy](../RECOGNITION.md)
- [Community Manager role](../roles/COMMUNITY_MANAGER.md) and
  [Reviewer role](../roles/REVIEWER.md)
