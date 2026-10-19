# Merge Strategy Policy

## Purpose

This policy defines how pull requests are merged into TeachLink Mobile's default
branch. A single, predictable strategy keeps `main` readable, makes each change easy
to find and revert, and keeps the history the release tooling reads accurate.

## Scope

This policy applies to every pull request merged into `main`, including `Governance/`
changes and dependency updates. It does not cover how contributors organise their
own branches before review, or releases and tags, which are governed by the
[Versioning Policy](VERSIONING.md) and the release process.

## Allowed Merge Strategies

| Strategy | Status | When to use |
| --- | --- | --- |
| Squash and merge | **Default** | Almost every pull request |
| Rebase and merge | Allowed, with conditions | A pull request whose commits are each meaningful, build on their own, and are worth keeping separately |
| Merge commit | **Not allowed** | Never, except under the documented exception below |

The [PR Approval Requirements](APPROVAL_REQUIREMENTS.md#branch-protection-rules)
already require linear history on `main`: merge commits must be rebased or squashed.
This policy makes that rule specific.

## Squash Rules

A squash merge turns the whole pull request into one commit on `main`. To keep that
commit useful:

- **Subject.** Use the pull request title, written as a
  [Conventional Commit](https://www.conventionalcommits.org/) such as `fix(auth): ...`
  or `feat(courses): ...`. The release workflow reads commit subjects to decide the
  version bump, so a subject that does not follow this form is invisible to it.
- **Body.** Summarise the change and link the issue it closes. Do not paste the list
  of intermediate commit messages ("fix typo", "address review").
- **Sign-off.** Keep every contributor's `Signed-off-by` line in the final message,
  as required by the [DCO Policy](DCO.md). If the squash drops them, the merger adds
  them back before merging.
- **Attribution.** When more than one person contributed, keep their
  `Co-authored-by` lines so the history credits them.
- **Signing.** A squash made in the GitHub web interface is signed by GitHub, which
  satisfies the [Commit Signing Policy](COMMIT_SIGNING.md) when it shows `Verified`.
  Merge from the web interface rather than from a local checkout.

## Linear History Rules

- `main` must contain no merge commits. Because squash and rebase both produce a
  straight line, following this policy preserves that automatically.
- Nobody pushes directly to `main`, and nobody force-pushes to it, regardless of role.
- A contributor may bring a branch up to date by rebasing it or by using the "Update
  branch" button. With a squash merge, a merge commit inside the branch disappears
  into the squashed commit. A branch merged by rebase must contain none.
- Rewriting history on a pull request branch (rebase, amend, force-push) is fine
  before merge, but resets approvals under the dismiss-stale-reviews rule, so the
  author says so in the pull request.

## Rebase and Merge Conditions

Rebase and merge is allowed only when **all** of the following are true:

1. Every commit follows the Conventional Commit format and carries a `Signed-off-by`
   line.
2. Each commit builds and passes the required checks on its own, so the history stays
   bisectable.
3. The pull request has no merge commits and no fix-up commits that should have been
   squashed.
4. The resulting commits will show as `Verified` under the
   [Commit Signing Policy](COMMIT_SIGNING.md). Rebasing rewrites commits, so a
   signature from the contributor's key does not carry over. If the resulting commits
   do not show `Verified`, squash instead.
5. A maintainer agrees in the pull request that the commits are worth preserving.

When in doubt, squash.

## Who Merges

A maintainer or committer merges, after the requirements in the
[Code Review Policy](REVIEW_POLICY.md) and the
[PR Approval Requirements](APPROVAL_REQUIREMENTS.md) are met: required checks green,
the required approvals present, blocking comments resolved. The merger chooses the
strategy using this policy and edits the squash message where it needs it. An author
who is a maintainer does not merge their own pull request unless a second maintainer
has approved it.

## Exceptions

| Situation | Handling |
| --- | --- |
| Release automation | `.github/workflows/release.yml` commits `chore(release)` and pushes directly to `main`. This is a recorded exception to the direct-push rule, not a precedent for people. Any change to that behaviour follows the [Commit Signing Policy](COMMIT_SIGNING.md) follow-up |
| A true merge commit is needed, such as preserving a long-running branch's full history | Requires approval from two maintainers, recorded in the pull request with the reason. The maintainers temporarily relax the linear-history rule and restore it immediately after the merge. Release tooling that ignores merge commits (`git log --no-merges`) will not see that merge |
| Emergency fix | Follows the [emergency override](APPROVAL_REQUIREMENTS.md#emergency-overrides). The merge strategy does not change: squash, from the web interface |
| Reverting a change | Use the web interface "Revert" button, which creates a pull request. It is squash-merged like any other change |

Every exception is recorded in the pull request that uses it and reviewed in the
monthly governance review.

## Enforcement

The repository is expected to be configured so that this policy is hard to break by
accident:

| Setting | Expected value |
| --- | --- |
| Allow merge commits | Off |
| Allow squash merging | On, default message: pull request title and description |
| Allow rebase merging | On |
| Require linear history on `main` | On (already listed in the approval requirements) |
| Restrict pushes to `main` | On |

These repository settings are not visible in the repository's files, so a
maintainer confirms them against the GitHub settings page at each monthly governance
review and records any difference. Where a setting is found off, it is corrected and
the correction is noted in the review.

## Related Documentation

- [Commit Signing Policy](COMMIT_SIGNING.md): which merge methods produce a verified
  commit on `main`
- [DCO Policy](DCO.md) and [Contributor and Release Sign-off Policy](CONTRIBUTOR_SIGNOFF.md):
  sign-off lines that must survive a squash
- [PR Approval Requirements](APPROVAL_REQUIREMENTS.md): branch protection and
  approvals
- [Code Review Policy](REVIEW_POLICY.md): the conditions for a mergeable pull request
- [Versioning Policy](VERSIONING.md) and [Changelog Policy](CHANGELOG_POLICY.md): why
  commit subjects matter to releases
