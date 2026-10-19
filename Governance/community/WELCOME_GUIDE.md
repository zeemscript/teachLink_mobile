# Newcomer Welcome Guide

## Purpose

This guide is the first page a new contributor to TeachLink Mobile should read. It
covers the first steps, the resources worth reading before starting, and where to get
help when something is unclear. The project's onboarding
[process](../processes/ONBOARDING.md) runs over weeks; this guide is the twenty minutes
that saves the first three.

## Scope

This guide is for someone who has found TeachLink Mobile and wants to contribute. It
covers getting oriented, running the app, making a first contribution, and finding help.
It does not set expectations for reviewers, describe the governance structures in
detail, or replace the [Contributor Agreement](CONTRIBUTOR_AGREEMENT.md).

## First Steps

Take these in order. Each one assumes the previous.

1. **Read the project's purpose.** [Vision](../VISION.md),
   [Scope](../SCOPE.md), and [Non-Goals](../NON_GOALS.md). The last one matters most: a
   well-written contribution that does something the project has decided not to do will be
   declined, and knowing that first is kinder than finding out at review.
2. **Read the Code of Conduct.** [Governance/COC_ENFORCEMENT.md](../COC_ENFORCEMENT.md)
   and the [reporting process](../COC_REPORTING.md). Participation is conditional on it.
3. **Read how to contribute.** [CONTRIBUTING.md](../../CONTRIBUTING.md) in the repository
   root, plus the [Contributor Agreement](CONTRIBUTOR_AGREEMENT.md), the
   [DCO Policy](../policies/DCO.md), and the
   [Contributor and Release Sign-off Policy](../policies/CONTRIBUTOR_SIGNOFF.md).
4. **Get the project running.** Clone, install dependencies, and start the app following
   `CONTRIBUTING.md`. Copy `.env.example` to `.env`; the example values point at a
   non-existent API, so expect the app to run but not to load data.
5. **Look around before changing anything.** `App.tsx`, `app/` for routes, `src/` for
   services and state, `components/` for shared UI, and
   `Governance/domains/` for the policies covering each area of the app.
6. **Pick something small and real.** See [Getting a First Task](#getting-a-first-task)
   below.
7. **Open a pull request early.** A small documentation fix is a complete first
   contribution, and the review on it teaches the review conventions before they matter.

## Key Resources

| Resource | Read it when |
| --- | --- |
| [CONTRIBUTING.md](../../CONTRIBUTING.md) | Before your first contribution — setup, workflow, and conventions |
| [README.md](../../README.md) | For what the app is and how it is built |
| [Vision](../VISION.md), [Scope](../SCOPE.md), [Non-Goals](../NON_GOALS.md) | Before proposing a feature |
| [Code of Conduct](../COC_ENFORCEMENT.md) | Once, before participating |
| [Governance README](../README.md) | To see how the project makes decisions |
| [Contributor Agreement](CONTRIBUTOR_AGREEMENT.md) | Before contributing code |
| [DCO Policy](../policies/DCO.md) and [sign-off](../policies/CONTRIBUTOR_SIGNOFF.md) | Before your first commit |
| [Code Review Policy](../policies/REVIEW_POLICY.md) | Before requesting a review |
| [Code Review Etiquette](REVIEW_ETIQUETTE.md) | If you are reviewing someone else's work |
| [First-Time Contributor Policy](../policies/FIRST_TIME_CONTRIBUTOR.md) | If you have never contributed to an open-source project |
| [Good-First-Issue Policy](../policies/GOOD_FIRST_ISSUE.md) | To find a suitably sized task |
| [Architecture principles](../PRINCIPLES.md) | To understand how the code is meant to be structured |
| [Glossary](../GLOSSARY.md) | When a governance term is unfamiliar |
| [Inclusive Language Guide](INCLUSIVE_LANGUAGE.md) | Whenever you write anything user-facing |

## Getting a First Task

Start smaller than feels too small.

- Issues labelled `good first issue` are sized for a first contribution, per the
  [Good-First-Issue Policy](../policies/GOOD_FIRST_ISSUE.md).
- Typo fixes, a missing test case, a documentation improvement, or a clearer error
  message are all real contributions. They are not practice.
- **Comment on the issue before you start** so two people do not write the same patch.
- **Say what you are picking up.** A short comment that you are looking at an issue
  prevents duplicate work and tells a maintainer to expect your pull request.
- **Ask for a mentor** if you would like one. The [Mentorship Program](MENTORSHIP_PROGRAM.md)
  explains how to ask and what to expect.

If nothing suitable is open, a documentation change in an area you already understand is
a good place to start.

## Making a First Contribution

1. **Branch from `main`.** One topic per branch.
2. **Make the smallest change that solves the problem.** Unrelated cleanups belong in
   their own pull request, because they make yours harder to review.
3. **Write a commit message a stranger can read.** The subject line is a summary; the
   body explains *why*, not *what* — the diff already shows what. See the
   [Changelog Policy](../policies/CHANGELOG_POLICY.md) for how changes are described.
4. **Sign off your commits.** `git commit -s`, as the
   [Contributor and Release Sign-off Policy](../policies/CONTRIBUTOR_SIGNOFF.md)
   requires, and confirm the [DCO](../policies/DCO.md) statement.
5. **Open a pull request** that says what problem it solves and how you verified it. A
   reviewer should not have to ask what the change is for.
6. **Close the issue** by referencing it, using `Closes #N` in the pull request body.
7. **Expect review, and treat it as part of the process.** Not every comment will be
   merged, and a disagreement is handled as described in the
   [Review Etiquette guide](REVIEW_ETIQUETTE.md) and the
   [Conflict Resolution Process](../processes/CONFLICT_RESOLUTION.md).

## What to Expect

Honest expectations up front:

- **Review takes time.** The [Code Review Service Level Agreement](../policies/REVIEW_SLA.md)
  gives the project's commitments. A large pull request waits longer.
- **Your first pull request will get comments.** That is what review is for. It is not a
  judgement of you.
- **Some requests will be declined.** A contribution can be good work and still be out
  of scope. The [Non-Goals](../NON_GOALS.md) document explains some of why, and a
  maintainer will explain the rest.
- **Merging is not the only outcome.** A well-reasoned issue that concludes "this is not
  something we want" is a useful contribution, and closing it politely is normal.
- **You do not need permission to start.** Ask for a `good first issue`, or pick
  something small. The project is open to contributions and the
  [Contributor Agreement](CONTRIBUTOR_AGREEMENT.md) is the agreement between you and the
  project.

## Where to Get Help

| For | Go to |
| --- | --- |
| Setup, build, or running the app locally | The issue tracker, per the [Support Policy](../policies/SUPPORT.md) |
| A question about how the project works | Discussions, or the documentation first |
| A pull request that has gone quiet | [Stale Pull Request Policy](../policies/STALE_PRS.md), then a friendly comment |
| Feedback on a proposal | The issue thread; proposals follow the [RFC Process](../processes/RFC_PROCESS.md) |
| A security problem | The private channel in the [Security Policy](../SECURITY_POLICY.md) — never a public issue |
| Harassment or a conduct concern | The [Code of Conduct Reporting](../COC_REPORTING.md) process |
| A governance question or a proposal to change governance | The [Escalation Path](../processes/ESCALATION_PATH.md) and the [Charter](../CHARTER.md) |
| Someone is stuck and unresponsive | The [Escalation Path](../processes/ESCALATION_PATH.md) |
| Language or accessibility support | Ask; it is arranged under the [Inclusive Language Guide](INCLUSIVE_LANGUAGE.md) and the onboarding process |
| Someone to help you directly | The [Mentorship Program](MENTORSHIP_PROGRAM.md) |
| Official recognition of your work | The [Contributor Recognition and Credits Policy](../RECOGNITION.md) |

If you are unsure where a question belongs, ask in the tracker and it will be redirected.
Asking in the wrong place is not a problem; not asking is.

## After Your First Contribution

- The [Contributor Onboarding Process](../processes/ONBOARDING.md) describes what happens
  in the following weeks, including a guided first contribution and a mentor.
- The [Role Promotion Criteria](../processes/PROMOTION_CRITERIA.md) explain how someone
  progresses towards reviewer and maintainer roles. The path is published, and it is not
  a queue.
- The [Contributor Recognition and Credits Policy](../RECOGNITION.md) explains how your
  work is credited.

## Maintaining This Guide

The [Community Manager](../roles/COMMUNITY_MANAGER.md) and the
[Docs Lead](../roles/DOCS_LEAD.md) maintain this guide. It is reviewed every 12 months
and whenever the onboarding process changes, because a welcome guide that describes the
old process is an obstacle rather than a welcome. Anyone may propose a change by opening
an issue or a pull request that touches only the `Governance/` folder.

## Related Documentation

- [Contributor Onboarding Process](../processes/ONBOARDING.md)
- [Mentorship Program](MENTORSHIP_PROGRAM.md) and
  [Ambassador Program](AMBASSADOR_PROGRAM.md)
- [First-Time Contributor Policy](../policies/FIRST_TIME_CONTRIBUTOR.md)
- [Good-First-Issue Policy](../policies/GOOD_FIRST_ISSUE.md)
- [Support Policy](../policies/SUPPORT.md) and
  [Security Policy](../SECURITY_POLICY.md)
- [Code of Conduct Enforcement](../COC_ENFORCEMENT.md) and
  [Code of Conduct Reporting](../COC_REPORTING.md)
- [Governance README](../README.md) and [Glossary](../GLOSSARY.md)
- [Community Manager Role](../roles/COMMUNITY_MANAGER.md) and
  [Documentation Lead Role](../roles/DOCS_LEAD.md)
