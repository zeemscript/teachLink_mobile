# Git Tagging Policy

## Purpose

This policy defines how releases are marked in Git: how tags are named, who may create
them, and the signing requirement. A tag is the permanent pointer from a published
version to the exact commit it was built from. Consistent, protected tags let anyone
reproduce a release, trace a store build back to source, and trust that a version
number never quietly changes meaning.

## Scope

This policy covers Git tags in this repository, and the GitHub releases attached to
them. It applies to release tags and pre-release tags. Version numbering rules
themselves belong to the [Versioning Policy](VERSIONING.md), and the release schedule to
the [Release Cadence](../processes/RELEASE_CADENCE.md); this policy only governs the
tag.

## Naming Convention

A tag name is the version name with a leading `v`.

| Kind | Format | Example | Defined by |
| --- | --- | --- | --- |
| Stable release | `vMAJOR.MINOR.PATCH` | `v1.16.4` | [Versioning Policy](VERSIONING.md) |
| Alpha pre-release | `vMAJOR.MINOR.PATCH-alpha.N` | `v1.17.0-alpha.1` | Versioning Policy, pre-release conventions |
| Beta pre-release | `vMAJOR.MINOR.PATCH-beta.N` | `v1.17.0-beta.2` | same |
| Release candidate | `vMAJOR.MINOR.PATCH-rc.N` | `v1.17.0-rc.1` | same |

Rules:

- The existing tags (`v1.6.0` through `v1.16.4`) follow the stable format, and new tags
  continue it.
- The version in the tag equals the `version` in `package.json` at the tagged commit. A
  tag that disagrees with `package.json` is a defect.
- `N` starts at 1 and increases within a stage. A tag name is never reused.
- No other tag names are created for releases. Tags that mark something other than a
  version, such as a build or a store submission, are not made; that information
  belongs in the release issue.

## Who May Tag

| Tag | Who may create it |
| --- | --- |
| Stable and release-candidate tags | The [Release Manager](../roles/RELEASE_MANAGER.md), or the release automation acting on the Release Manager's behalf |
| Alpha and beta tags | The Release Manager, or a [Maintainer](../roles/MAINTAINER.md) |
| A tag on a security or hotfix release | The Release Manager with the [Security Officer](../roles/SECURITY_OFFICER.md) for security fixes, or a Maintainer for critical bugs, as set out in the Release Manager's sign-off authority |

Nobody else tags. A contributor who needs a release marked asks in the release issue.
Tags are created only on a commit that is already on `main` or on a supported release
branch (see [Release Branching](RELEASE_BRANCHING.md)), after the required checks and
approvals for that commit have passed.

## Tag Contents

A release tag is an **annotated** tag. Its message states:

- the version and the date;
- the release issue it belongs to; and
- the commit it was built from, when the tag is created from a release branch.

Annotated tags carry an author, a date, and a message, and can be signed. Lightweight
tags carry none of these and are not used for releases.

## Signing Requirement

Release tags are **signed** by the person who creates them, with a key registered to
their GitHub account, so that anyone can verify who published a version:

```bash
git tag -s v1.17.0 -m "Release v1.17.0 (release issue #NNNN)"
git tag -v v1.17.0
git push origin v1.17.0
```

This is the release-tag requirement of the [Commit Signing Policy](COMMIT_SIGNING.md), and
the key rules there apply. A signed tag does not replace the commit-level checks already
required for merging, and it is different from the signing keys used for app builds, which are governed by
[App Signing Keys](../domains/APP_SIGNING_KEYS.md).

### Enforcement status

This section states what is and is not true today, so the policy is not mistaken for a
description of a control that exists.

- The existing tags are **lightweight**, not annotated: for example `v1.6.0` through
  `v1.9.0` point directly at a commit with no tag object. They carry no message and no
  signature.
- `.github/workflows/release.yml` creates the GitHub release and its tag with
  `softprops/action-gh-release`, using `tag_name: v<version>`. That step does not
  create a signed, annotated tag.

So the signing requirement applies from the date this policy is adopted and is **not yet
met by the release automation**. Until the workflow is changed, the Release Manager
creates the signed annotated tag by hand before publishing the release, and records that
in the release issue. Changing the automation is a follow-up owned by the Release
Manager.

## Immutability

A pushed tag is permanent.

- A tag is never moved, re-pointed, force-pushed, or deleted to "fix" a release.
- If a release is wrong, a new patch version is released with a new tag. The wrong
  release is marked as superseded in its release notes.
- A tag may be deleted only if it exposes something that must not be public, such as a
  secret committed by mistake, or is a legal requirement. That needs two Maintainers and
  the Security Officer, and the reason and the actions taken are recorded in a restricted
  issue. The version number is not reused.

Maintainers should enable GitHub tag protection for the pattern `v*` so that creating,
updating, and deleting these tags is restricted to the roles above. As with other
repository settings, this is confirmed at each release.

## Releases and Tags Together

Every published GitHub release has exactly one tag, and every stable tag has a GitHub
release. The release notes follow the [Changelog Policy](CHANGELOG_POLICY.md). A
pre-release tag is marked as a pre-release on GitHub and is never advertised as a stable
version.

## Ownership

The release-management team owns this policy. Changes are proposed in a pull request
that touches only the `Governance/` folder.

## Related Documentation

- [Versioning Policy](VERSIONING.md) and [Changelog Policy](CHANGELOG_POLICY.md)
- [Release Branching Policy](RELEASE_BRANCHING.md)
- [Commit Signing Policy](COMMIT_SIGNING.md)
- [Release Cadence](../processes/RELEASE_CADENCE.md)
- [Release Manager role](../roles/RELEASE_MANAGER.md)
- [App Signing Keys](../domains/APP_SIGNING_KEYS.md)
