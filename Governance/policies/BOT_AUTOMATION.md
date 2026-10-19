# Bot and Automation Governance Policy

## Purpose

This policy defines which automations may act on the TeachLink Mobile repository, what
permissions each may hold, and how they are reviewed and audited. A bot or workflow runs
with real credentials and acts without a person watching each step, so it is held to a
stricter standard than a person: it gets the least access that does its job, and its
access is reviewed on a schedule.

## Scope

This policy covers every non-human actor that can read, comment on, commit to, label,
merge, or release from this repository, or that holds a secret belonging to it. That
includes GitHub Actions workflows, dependency-update bots, GitHub Apps, and personal or
machine access tokens used by automation. It does not cover the tools contributors run
on their own machines.

## Allowed Automations

| Automation | What it may do | Where it is defined |
| --- | --- | --- |
| **CI and quality workflows** | Run lint, typecheck, tests, architecture and dependency checks, bundle-size and performance checks, and report results | `.github/workflows/ci.yml`, `architecture.yml`, `audit.yml`, `bundle-size.yml`, `performance-regression.yml`, `monitor-build-times.yml` |
| **Build workflows** | Build native and web artifacts | `.github/workflows/build-native.yml`, the EAS jobs in `release.yml` |
| **Release automation** | Bump the version, update the changelog, create the release, and start production builds | `.github/workflows/release.yml` |
| **Dependency updates** | Open pull requests that update dependencies, and merge a narrow class of them | `renovate.json` |

Any other automation, such as an auto-labeller, a stale-item closer, a welcome message
bot, or a merge queue helper, is **not allowed until it is added to this table** by a
pull request that follows [Adding or Changing an Automation](#adding-or-changing-an-automation).
Inactivity handling in particular stays with people, as the
[Stale Pull Request Policy](STALE_PRS.md) requires: pull requests are not closed by an
inactivity bot.

## Rules for Every Automation

1. **Least privilege.** An automation gets only the permissions it needs, for only as
   long as it needs them.
2. **Named owner.** Each automation has an accountable owner (a role, not an
   individual), recorded in the table in [Ownership](#ownership).
3. **No secrets in code.** Credentials live in repository or environment secrets, never
   in the repository, logs, issue text, or workflow output.
4. **Never the sole reviewer.** An automation does not approve a pull request, and it does
   not satisfy the human approval requirements in the
   [PR Approval Requirements](APPROVAL_REQUIREMENTS.md).
5. **Identifiable.** An automation acts under a recognisable bot identity, so its actions
   are distinguishable from a person's in history and audit logs.
6. **Auditable.** What it did can be traced to a workflow run, a commit, or a pull request.
7. **Failure is safe.** If an automation fails, it stops. It does not retry destructive
   actions or fall back to broader credentials.

## Permissions

### Workflows

- Every workflow declares a top-level `permissions:` block and defaults to
  **read-only** (`contents: read`). The existing `ci.yml` and `audit.yml` follow this
  rule, which was adopted to restrict the default `GITHUB_TOKEN`.
- Write permissions are granted **per job**, never at workflow level, and only for the
  scope required. For example, only the release jobs need `contents: write` and
  `pull-requests: write`.
- Jobs that receive production secrets (`EXPO_TOKEN`, `MAESTRO_API_KEY`, or any signing
  material) run only on trusted refs, never on pull requests from forks, and are skipped
  rather than failed when the secret is absent.
- Workflows triggered by `pull_request_target`, or that check out code from a fork while
  holding secrets, are **not allowed**, because they run untrusted code with trusted
  access.
- Third-party actions are pinned to an immutable reference (a full commit SHA) and
  reviewed before they are added. Actions that handle secrets or write to the repository
  receive the most careful review.

> **Current state.** The existing workflows reference third-party actions by version
> tag (for example `softprops/action-gh-release@v2`), not by commit SHA. Pinning them is a
> follow-up owned by the Release Manager and the Security Officer; new actions are pinned
> from the date this policy is adopted.

### Dependency bot

- It may open pull requests and push to its own branches. It may not push to `main`.
- It may **automerge only** patch updates of development dependencies, and only when the
  required `ci` check passes. This is the single automerge class configured in
  `renovate.json`. Any widening of automerge needs maintainer approval under the process
  below.
- Updates to `expo`, `react-native`, and other platform-critical packages are grouped and
  reviewed by a person; the bot does not merge them.

### Release automation

- It is the only automation allowed to write to `main`: `release.yml` commits the version
  bump and changelog and pushes them. This is a recorded exception to the rule that
  nobody pushes directly to `main` (see the [Merge Strategy Policy](MERGE_STRATEGY.md)).
- It may create tags and releases. Tags and the commit it pushes are subject to the
  signing requirements in the [Commit Signing Policy](COMMIT_SIGNING.md), and the
  current gaps are tracked there.
- Store submission is gated by an explicit input and by the release sign-off in the
  [Contributor and Release Sign-off Policy](CONTRIBUTOR_SIGNOFF.md); it never submits
  silently.

### Tokens and Apps

- Prefer the workflow's built-in `GITHUB_TOKEN`. A personal access token is used only
  when the built-in token cannot do the job, is a fine-grained token limited to this
  repository, has an expiry, and is not owned by a person who may leave the project.
- A GitHub App or bot account is installed with the narrowest permission set and is
  listed, with its permissions, in the register below.

## Review and Audit

| Cadence | Who | What |
| --- | --- | --- |
| On every change to a workflow or bot config | A [Maintainer](../roles/MAINTAINER.md) and the [Security Officer](../roles/SECURITY_OFFICER.md) | Review permissions, secrets used, triggers, and new third-party actions |
| Monthly, in the governance review | Maintainers | Check the register against the installed Apps, tokens, and workflows; confirm no unlisted automation exists |
| Quarterly | Security Officer | Review the secrets and tokens held by automation, rotate any that are old or unused, and revoke any that are no longer needed |
| After any incident | Security Officer | Review what the automation could reach; reduce access where the incident showed it was too broad |

A review records its outcome, including "no change needed". A permission that has not
been used in a review period is removed.

## Adding or Changing an Automation

1. Open an issue describing the automation, why it is needed, what it does, the exact
   permissions and secrets it needs, and what happens if it malfunctions.
2. Propose the change in a pull request that updates this policy's tables and the
   workflow or configuration together.
3. The Security Officer and a Maintainer review it. An automation that writes to the
   repository, holds a secret, or can merge needs both to approve.
4. After merge, the owner adds it to the register and confirms the permissions in the
   live configuration match the approved ones.

## Incidents

If an automation is compromised, misbehaves, or acts outside its remit:

1. The Security Officer or any Maintainer disables it immediately (disable the workflow,
   uninstall the App, or revoke the token).
2. Credentials it held are rotated under the Security Officer.
3. The actions it took are reviewed and reverted where necessary.
4. The cause is recorded and handled under the [Security Policy](../SECURITY_POLICY.md)
   and the [Escalation Path](../processes/ESCALATION_PATH.md).

## Ownership

| Automation | Accountable role |
| --- | --- |
| CI and quality workflows | Maintainers |
| Build workflows | [Release Manager](../roles/RELEASE_MANAGER.md) |
| Release automation | Release Manager, with the Security Officer for credentials |
| Dependency bot | Maintainers |

This policy is owned by the Maintainers. Changes are proposed in a pull request that
touches only the `Governance/` folder.

## Related Documentation

- [PR Approval Requirements](APPROVAL_REQUIREMENTS.md)
- [Merge Strategy Policy](MERGE_STRATEGY.md) and [Commit Signing Policy](COMMIT_SIGNING.md)
- [Stale Pull Request Policy](STALE_PRS.md)
- [Security Policy](../SECURITY_POLICY.md)
- [Security Officer role](../roles/SECURITY_OFFICER.md)
