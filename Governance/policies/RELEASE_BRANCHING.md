# Release Branching Policy

## Purpose

This policy defines the branching model for releases: when a release branch is cut, how
a release candidate is stabilised, and how a fix reaches an older supported line. It
keeps `main` open for ongoing work while a candidate is being hardened, and gives
security fixes a reliable path to the lines users are still running.

## Scope

This policy covers `main` and the `release/*` branches, and the flow of changes between
them. It works with the [Release Cadence](../processes/RELEASE_CADENCE.md) (the
two-week train and the 48-hour freeze), the [Versioning Policy](VERSIONING.md), the
[Tagging Policy](TAGGING.md), and the supported-lines rule in the
[Security Policy](../SECURITY_POLICY.md). It does not cover how contributors organise
their own feature branches.

## Branching Model

The project is **trunk-based**. `main` is the integration branch and the source of every
release. Release branches are short-lived and exist only to stabilise a candidate or to
maintain a supported older line.

| Branch | Purpose | Lifetime |
| --- | --- | --- |
| `main` | All development. Releases are cut from it | Permanent |
| `release/MAJOR.MINOR` | Stabilisation of a candidate, then maintenance of that line while it is supported. Example: `release/1.17` | Until the line is no longer supported |
| Feature and fix branches | One change, opened as a pull request to `main` | Until merged |

There are no long-lived `develop` or per-feature release branches. A release branch is
named after the minor line (`release/x.y`), as the
[Release Manager role](../roles/RELEASE_MANAGER.md) already describes.

All branches that receive merges follow the same rules as `main`: changes arrive through
a pull request with the required checks and approvals, and nobody pushes or force-pushes
directly. Releases are merged by squash or rebase as set out in the
[Merge Strategy Policy](MERGE_STRATEGY.md), which keeps history linear.

## When a Release Branch Is Cut

A release branch is cut when a candidate is frozen for stabilisation, which is the start
of the final 48 hours before promotion described in the Release Cadence. The
[Release Manager](../roles/RELEASE_MANAGER.md) cuts it, with the Maintainers notified.

### Cut steps

1. Confirm that the release issue lists the candidate scope and the commit on `main` to
   cut from, and that required checks are green on that commit.
2. Create the branch from that exact commit:

   ```bash
   git switch main && git pull --ff-only
   git switch -c release/1.17 <candidate-commit>
   git push origin release/1.17
   ```

3. Record the branch and commit in the release issue.
4. Announce the freeze. From now on `main` accepts the next train's work, and the
   candidate changes only on the release branch.

A patch release does not need a new branch. It is made on the existing `release/x.y`
branch for that line.

## Stabilisation

While the candidate is on the branch:

- Only release-blocking fixes are accepted: security, privacy, build, and data-integrity
  problems, in line with the freeze rules in the Release Cadence. Features and
  refactors wait for the next train.
- Each fix **lands on `main` first**, and is then brought onto the release branch (see
  [Backport Flow](#backport-flow)). A fix that exists only on a release branch is a
  defect, because the next release would lose it.
- Release-candidate builds are tagged `vX.Y.Z-rc.N` from the branch tip, as set out in
  the [Tagging Policy](TAGGING.md), and validated through the required internal, beta,
  and store channels.
- Every change on the branch needs the release owner's approval and a fresh validation
  pass, as the freeze rules require.

### Promotion

When the Release Manager and the Maintainers approve the candidate, the Release Manager
tags the branch tip `vX.Y.Z` and publishes the release. The `package.json` version on the
branch at that commit matches the tag. The branch then stays as the maintenance line for
`X.Y`.

## Backport Flow

A fix reaches a supported older line by being **cherry-picked from `main`**, never by
merging the release branch into `main`.

1. **Fix on `main` first.** The fix merges to `main` through the normal process.
2. **Decide to backport.** The fix is backported only if it is a security fix, a
   data-loss or privacy fix, or a build-breaking fix for a supported line. The line must
   be supported: the current line, and the previous line only while the current line is
   less than two weeks old, as the Security Policy states. Features are never backported.
3. **Label.** Add `backport/X.Y` to the merged pull request.
4. **Cherry-pick** onto a branch from `release/X.Y`, keeping a reference to the original:

   ```bash
   git switch -c backport/1.16-fix-token-refresh release/1.16
   git cherry-pick -x <commit-on-main>
   ```

   The `-x` flag records the source commit in the message. Keep any `Signed-off-by`
   lines; add `-s` if cherry-picking would otherwise drop them.
5. **Pull request** to `release/X.Y`, with the same checks and approvals as for `main`,
   linking the original pull request.
6. **Patch release.** The Release Manager tags the next patch version on the branch
   (for example `v1.16.5`) after the Security Officer signs off a security fix.

A backport that does not apply cleanly is reworked on the release branch and the
difference from the original is explained in the pull request.

## Ending a Line

When a line stops being supported under the Security Policy, the Release Manager records
that in the release issue and removes the `release/X.Y` branch protection and branch.
Its tags and releases are kept permanently.

## Exceptions

An emergency fix follows the [emergency override](APPROVAL_REQUIREMENTS.md#emergency-overrides)
in the approval requirements. It may skip nothing about branch flow: it still lands on
`main` first and is then backported, with the review window shortened rather than the
order changed.

## Current state

This section records what is true today so the policy is not read as describing
machinery that exists.

- The repository has no `release/*` branches. All releases so far have been made from
  `main`.
- `.github/workflows/release.yml` runs on pushes to `main` and bumps the version, creates
  the release, and builds from there. It does not run for `release/*` branches.
- Branch protection for `release/*` is not configured by anything in the repository.

Adopting this policy therefore means the Release Manager cuts and protects release
branches by hand when the first candidate needs one, and the release workflow is
extended to handle `release/*` as a follow-up owned by the Release Manager. Until then,
a release that does not need stabilisation may continue to be cut from `main` directly.

## Ownership

The release-management team owns this policy. Changes are proposed in a pull request
that touches only the `Governance/` folder.

## Related Documentation

- [Release Cadence](../processes/RELEASE_CADENCE.md)
- [Versioning Policy](VERSIONING.md) and [Tagging Policy](TAGGING.md)
- [Merge Strategy Policy](MERGE_STRATEGY.md)
- [Security Policy](../SECURITY_POLICY.md)
- [Release Manager role](../roles/RELEASE_MANAGER.md)
