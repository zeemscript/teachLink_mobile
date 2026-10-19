# Commit Signing Policy

## Purpose

This policy defines when a commit on TeachLink Mobile's default branch must carry a
verified cryptographic signature, how that is checked, and how the keys behind it are
managed. A signature proves a commit was made by the holder of a registered key, so
that the history of the branch the app is built from cannot be forged by anyone who
merely knows a maintainer's name and email address.

## Scope

This policy applies to commits that land on `main`, to release tags, and to the keys
and accounts that produce them. It covers the commits of maintainers, committers, and
release managers, the commits created by GitHub when a pull request is merged, and
automation that pushes to the repository.

It is separate from, and in addition to, the `Signed-off-by` line required by the
[Contributor and Release Sign-off Policy](CONTRIBUTOR_SIGNOFF.md) and the
[Developer Certificate of Origin Policy](DCO.md). The two answer different questions:

| Mechanism | Question it answers | Who it binds |
| --- | --- | --- |
| `Signed-off-by` trailer (DCO) | Does the contributor have the right to submit this work? | Every contributor, every commit |
| Cryptographic signature (this policy) | Was this commit really made by the holder of the key? | Anyone producing a commit that lands on `main` |

A sign-off is plain text that anyone can type. A signature cannot be produced without
the private key. Neither replaces the other. It also does not cover the keys that
sign app builds, which are governed by [`../domains/APP_SIGNING_KEYS.md`](../domains/APP_SIGNING_KEYS.md).

## Signing Requirement

| Commit or object | Requirement |
| --- | --- |
| A commit authored by a maintainer, committer, release manager, or security officer | **Must** be signed and show as `Verified` on GitHub |
| A commit on a contributor's pull request branch | Signing is **recommended**, not required |
| The commit that lands on `main` through a squash merge made in the GitHub web interface | Created and signed by GitHub, and must show as `Verified` |
| A rebase merge to `main` | Allowed only when every resulting commit shows as `Verified`; otherwise squash instead (see the [Merge Strategy Policy](MERGE_STRATEGY.md)) |
| A release tag | **Must** be an annotated tag signed by the release manager |
| A commit pushed by automation | Must be signed through a GitHub-managed identity, or recorded as an exception below |

The reason contributors are not required to sign is practical: a squash merge replaces
their commits with one new commit, so their individual signatures would not survive on
`main` in any case. The protection that matters is that everything on `main` is
verifiably attributable to either a registered key or to GitHub itself.

A signature is only meaningful if the commit email matches a verified email on the
signer's GitHub account. A signature that GitHub reports as `Unverified`, including
because the key is unknown, expired, revoked, or the email does not match, does not
satisfy this policy.

## Verification in CI

Verification relies on GitHub's own verdict, exposed as the `verification.verified`
field of each commit, rather than on checking signatures inside the workflow. This
avoids shipping public keys to CI and keeps the trust anchor in one place.

Two checks are specified:

| Check | When it runs | What it does | On failure |
| --- | --- | --- | --- |
| Pull request check | On every pull request | For each commit authored by a maintainer-level account, requires `verification.verified` to be `true`. Other commits are reported but do not fail the check | Fails the pull request and names the unverified commits |
| Default-branch audit | After each push to `main`, and on a weekly schedule | Requires `verification.verified` to be `true` for every commit added since the last audit | Opens an issue labelled `security` and notifies the security officer |

An illustrative query, which a workflow would wrap in a loop over the pull request's
commits:

```bash
gh api "repos/{owner}/{repo}/commits/$SHA" --jq '.commit.verification.verified'
```

### Enforcement status

This section states plainly what is and is not enforced today, because a policy that
describes a control that does not exist is worse than none.

- None of the workflows under `.github/workflows/` inspects commit signatures. The
  only `gpg` use is in `performance-regression.yml`, to install the k6 package
  repository key.
- The branch protection rules listed in the
  [PR Approval Requirements](APPROVAL_REQUIREMENTS.md#branch-protection-rules) do not
  include "Require signed commits".
- `.github/workflows/release.yml` creates a `chore(release)` commit and pushes it
  directly to `main` using `git commit` with no signing option. As written, that
  commit would not be signed by a registered key.

The controls below are therefore a follow-up obligation, not a description of current
behaviour:

| Item | Owner | Target |
| --- | --- | --- |
| Add the pull request signature check and the default-branch audit described above | [Security Officer](../roles/SECURITY_OFFICER.md) with the maintainer team | Before the next release, reviewed each release until in place |
| Enable "Require signed commits" on `main` once the release automation signs its commit | Maintainer team | After the release workflow is changed |
| Make the release workflow sign its commit and tag through a GitHub-managed or dedicated signing identity | [Release Manager](../roles/RELEASE_MANAGER.md) | Alongside the first two items |

Until the automated checks exist, a maintainer confirms signatures by hand: before
merging, check the commit list for `Verified` badges on every maintainer-authored
commit, and before publishing a release, run `git tag -v <tag>`.

## Key Management

### Accepted keys

| Key type | Status | Notes |
| --- | --- | --- |
| SSH signing key (Ed25519) | Preferred | Simple to create and rotate; registered on GitHub as a **signing key**, which is separate from an authentication key |
| GPG key (Ed25519 or RSA 4096) | Accepted | Must have an expiry no more than two years out |
| S/MIME certificate | Accepted | Only where an organisational certificate already exists |

Weaker keys, such as RSA below 3072 bits or DSA, are not accepted.

### Storage and handling

- Maintainers should keep the private key on a hardware token or in the platform
  secure enclave. If a software key is used, it must be protected by a passphrase.
- A private key must never be committed, pasted into an issue or pull request, shared
  between people, or stored as a repository or CI secret.
- Each person uses their own key. A shared or team key is not permitted, because the
  signature would no longer identify an individual.

### Registration

The signer adds the public key to their GitHub account as a signing key and confirms
that the email used in `user.email` is a verified address on that account. A
GitHub-provided `noreply` address is acceptable.

### Rotation, expiry, and revocation

| Event | Required action | Deadline |
| --- | --- | --- |
| Key expires or is due for rotation | Generate a new key, register it, then remove the old one | Before expiry |
| Key is lost, or suspected or known to be compromised | Remove it from the GitHub account, notify the security officer, and review every commit and tag signed with it since the last known-good date | Within 24 hours |
| Holder leaves a maintainer role | Offboarding removes the holder's elevated access; the holder removes the signing key from the account if the account is kept | At offboarding, per [`../processes/OFFBOARDING.md`](../processes/OFFBOARDING.md) |
| Key is rotated | Previously signed commits stay valid in history; do not rewrite history to re-sign them | n/a |

A compromise is handled as a security incident under the
[Security Policy](../SECURITY_POLICY.md). The review in the second row looks for
commits the key signed that the holder did not make.

## Signing an Existing Branch

To sign the commits on a branch before review, rebase and re-sign:

```bash
git rebase --exec 'git commit --amend --no-edit -S' <base>
git push --force-with-lease
```

where `<base>` is the merge base of the branch and the default branch. This rewrites
history, so every commit gets a new identifier and any `Signed-off-by` lines must
survive; add `--signoff` to the amend command if they would otherwise be lost, as
described in the [DCO policy](DCO.md). Reviewers must re-review after a force-push.

## Exceptions

| Situation | Handling |
| --- | --- |
| Release automation committing to `main` | Recorded exception until the release workflow signs its commit; tracked in the follow-up table above |
| Commits made by a GitHub App or the GitHub web interface | Signed by GitHub; treated as satisfying this policy when they show `Verified` |
| An emergency fix under the [emergency override](APPROVAL_REQUIREMENTS.md#emergency-overrides) | A maintainer-authored commit still must be signed. If a signing key is unavailable, a second maintainer may merge via the web interface, which signs the resulting commit |
| A contributor unable to sign | Not an exception: contributors are not required to sign |

An exception is recorded in the pull request or issue that relies on it, with the
reason and the person who approved it. Exceptions are reviewed in the monthly
governance review.

## Related Documentation

- [Merge Strategy Policy](MERGE_STRATEGY.md): which merge methods produce a signed
  commit on `main`
- [Contributor and Release Sign-off Policy](CONTRIBUTOR_SIGNOFF.md) and the
  [DCO Policy](DCO.md): the text sign-off that this policy complements
- [PR Approval Requirements](APPROVAL_REQUIREMENTS.md): branch protection rules
- [App Signing Key Policy](../domains/APP_SIGNING_KEYS.md): the separate keys that sign
  app builds
- [Security Officer role](../roles/SECURITY_OFFICER.md) and
  [Release Manager role](../roles/RELEASE_MANAGER.md)
