# CI/CD Governance Policy

## Purpose

This policy defines who may change the automation that builds, tests, audits, and
releases TeachLink Mobile, what secrets and permissions those workflows are allowed
to hold, and which automated results may be relied on to gate a merge. The workflows
under `.github/workflows/` can read source code, run third-party actions, publish
artifacts, and cut releases, so changes to them are treated as privileged changes
rather than routine maintenance.

## Scope

This policy covers the workflow definitions in `.github/workflows/`, the actions they
call, the job and step structure within them, the permissions and secrets granted to
each job, and the status checks used to gate a merge into `main`.

It covers the eight workflows in the repository today:

| Workflow | Trigger | Responsibility |
| --- | --- | --- |
| `ci.yml` | Pull request, push to `main` | Font subsetting, lint, format check, typecheck, unit and contract tests, web build, bundle size, API performance |
| `architecture.yml` | Pull request, push | Dependency boundary enforcement through `dependency-cruiser` |
| `audit.yml` | Push, weekly schedule | High and critical dependency advisories, unused dependencies, disallowed licenses |
| `bundle-size.yml` | Pull request, push | Bundle size trend tracking |
| `build-native.yml` | Manual, pull request | Native iOS and Android builds with dependency caching |
| `performance-regression.yml` | Pull request, push, schedule | k6 performance budgets |
| `monitor-build-times.yml` | Schedule | CI build duration monitoring |
| `release.yml` | Tag or manual | Release preparation, changelog, and tag publication |

Build configuration for the mobile artifacts themselves — profiles, signing, and store
submission — is owned separately by the [EAS Build Governance](../domains/EAS_BUILD_GOVERNANCE.md)
and the [App Store Release Governance](../domains/APP_STORE_RELEASE.md). This policy
governs the pipelines, not the product settings those pipelines read.

## Who May Change Pipelines

Pipeline changes are reviewed by people who can be held responsible for them.

| Change | Who may propose | Required review |
| --- | --- | --- |
| Fix a typo, comment, or job name in a workflow | Any contributor | 1 maintainer |
| Add or adjust a lint, typecheck, test, or build step that runs no new third-party code | Maintainer | 1 maintainer |
| Change a required check, a job's `if` condition, or a path filter so that a check stops running on some changes | Maintainer | 2 maintainers |
| Add a third-party action, a new external download, or a step that runs code from outside the repository | Maintainer | 2 maintainers, one of whom is a [Release Manager](../roles/RELEASE_MANAGER.md) or [Maintainer](../roles/MAINTAINER.md) |
| Change `release.yml`, or anything that creates tags, publishes releases, or pushes to `main` | Maintainer | 2 maintainers including the [Release Manager](../roles/RELEASE_MANAGER.md) |
| Grant a workflow additional `permissions:` or a new secret | Maintainer | 2 maintainers and the [Security Officer](../roles/SECURITY_OFFICER.md) |
| Add a GitHub App, bot, or personal access token used by CI | Maintainer | 2 maintainers and the Security Officer |

Nobody may merge a workflow change to `main` that they authored alone. This holds even
for maintainers: the [Maintainer Override](../policies/APPROVAL_REQUIREMENTS.md#maintainer-override)
that permits a maintainer to override approval requirements for a CI/CD fix still
requires that all required checks pass, and the [Decision Rights Matrix](../matrices/DECISION_RIGHTS_MATRIX.md)
reserves the release pipeline to the release-management group.

Automation that proposes its own changes is out of scope for self-approval. A bot
opened pull request follows the same review requirements as a human one, under the
[Bot and Automation Governance Policy](BOT_AUTOMATION.md).

### Review obligations for pipeline changes

A reviewer of a pipeline change is responsible for reading the diff as executable code,
not as configuration. Specifically, the reviewer confirms:

- the change does not widen token permissions, add a secret reference, or introduce a
  network destination that the previous version did not have;
- no `pull_request_target` context is used to check out and run untrusted code;
- third-party actions are pinned to a full commit SHA, not to a floating tag or branch;
- any new script executed by a workflow exists in the repository and is readable; and
- the change is covered by the required checks below.

A reviewer who cannot verify a third-party action's behaviour declines the review rather
than approving on trust.

## Secret and Permission Rules

### Secrets

- Secrets are never written into a workflow file, a workflow's `env:` block, a command
  line, or a committed default. Workflows reference a secret by name only.
- A secret is scoped to the narrowest thing that needs it: a single workflow, then a
  single job, then a single environment. A secret defined at workflow level is
  available to every job in it, so job or environment scope is preferred.
- No secret value is echoed, printed, or interpolated into a command that writes to a
  log. Build output that will be shared outside the maintainer team is read for
  accidental disclosure first, as required by the
  [EAS Build Governance](../domains/EAS_BUILD_GOVERNANCE.md).
- Pull requests from forks never receive secrets. Workflows that need a secret are
  guarded so that the secret-bearing step does not run on `pull_request` events from
  forks, and `pull_request_target` is never used as a workaround because it runs with
  repository privileges on an untrusted checkout.
- A secret suspected of exposure is rotated immediately rather than investigated to
  completion first, under the cadence in the
  [Secret Rotation Policy](SECRET_ROTATION.md), and the exposure is reported through
  the [Security Policy](../SECURITY_POLICY.md).
- Contributor and maintainer credentials, including personal access tokens, are not
  stored as CI secrets for convenience. Automation uses a GitHub App with narrow
  permissions instead.

### Permissions

Workflows declare `permissions:` explicitly at the top level and narrow it further at
the job level. The default is the minimum the job needs, and `contents: write` is
granted only to a job that genuinely commits, tags, or publishes.

| Capability | Where it is allowed |
| --- | --- |
| `contents: read` | Any workflow; this is the default ceiling for checkout-only work |
| `contents: write` | Release and tag publication jobs only |
| `pull-requests: write` | Jobs that label or comment on pull requests |
| `packages: write` | None today; added only with Security Officer approval |
| `id-token: write` | None today; added only with Security Officer approval, and only with a recorded reason |

Omitting the `permissions:` block is a finding, not a shortcut: a workflow inherits
the repository's default token permissions, which are not visible in the file that
uses them.

### Third-party code

- Third-party actions are referenced by full commit SHA with the version in a trailing
  comment, for example `uses: actions/checkout@<sha> # v4`.
- A new third-party action is added only when the maintainers agree the dependency is
  necessary, and the choice is recorded in the
  [Dependency Register](../registers/DEPENDENCY_REGISTER.md).
- Downloaded artifacts and installed packages are not trusted merely because a step
  succeeded. Where a workflow curls a file, the maintainers prefer a pinned version
  with a checksum.

## Required-Check Policy

A merge into `main` requires the results of the checks below. The set is the union of
what `ci.yml` and `architecture.yml` run on every pull request and the list in the
[PR Approval Requirements](../policies/APPROVAL_REQUIREMENTS.md#required-checks);
the approval document is authoritative if the two ever disagree, and correcting it is
part of the change that resolves a disagreement.

| Check | Gate | Notes |
| --- | --- | --- |
| Lint | Required | `npm run lint`, within the warning budget in `lint-budget.json` |
| Format check | Required | `npm run format:check` |
| Typecheck | Required | `tsc --noEmit` |
| Test | Required | Jest unit suite |
| OpenAPI validation | Required | `npm run validate:openapi` |
| Contract tests | Required | The `openapi.contract` and `validation` suites |
| Bundle size check | Required | `scripts/checkBundleSize.js` against `performance-budget.json` |
| API performance check | Required | `scripts/checkApiPerf.js` |
| Architecture boundaries | Required | `dependency-cruiser` over `src`, `app`, and `components` |
| Dependency audit | Required before merge of a dependency change | `audit.yml`, high and critical advisories |
| Native build | Required for changes under native paths | `build-native.yml` |
| End-to-end tests | Required before release, advisory on pull requests | Maestro flows in the `test-e2e` job |

### Rules the set must satisfy

- **No silent removal.** A change that stops a required check from running on a class
  of pull requests is treated as a security-relevant change and needs two maintainer
  reviews. Making a check permanently advisory, deleting it, or narrowing the `paths:`
  filter that triggers it so that it no longer covers security-relevant code are all
  removals under this rule.
- **No self-approval.** A check run by the same change it gates does not satisfy this
  policy. A workflow that lints the repository but is itself excluded from linting by a
  path filter must have that filter corrected.
- **No retry substitution.** Re-running a failed check is allowed and does not count as
  a pass on its own; a check that fails intermittently is a defect in the check and is
  fixed or recorded as an accepted exception.
- **Always-green is not the goal.** A check that is permanently passing because it
  tolerates failure provides no gate. Checks that cannot be made meaningful in
  continuous integration are moved to a scheduled workflow and documented there rather
  than left silently passing on pull requests.
- **Failure to report is failure.** A check that cannot run because of infrastructure,
  such as a runner outage or a rate limit, does not pass the pull request. The
  [Escalation Path](../processes/ESCALATION_PATH.md) applies if it persists.

### Enforcement status

- Branch protection on `main` is described in the
  [PR Approval Requirements](../policies/APPROVAL_REQUIREMENTS.md#branch-protection-rules);
  whether "Require status checks to pass" is enabled in the repository settings is not
  verifiable from the repository itself and is confirmed by a maintainer.
- `.github/workflows/` declares `permissions:` in `ci.yml`, `audit.yml`, and
  `release.yml`. `architecture.yml`, `bundle-size.yml`, `build-native.yml`,
  `monitor-build-times.yml`, and `performance-regression.yml` do not, so those four
  inherit the repository default.
- No `.github/CODEOWNERS` file exists, so this policy's review requirements are
  enforced by convention and by the approval rules rather than by GitHub.

Until branch protection is confirmed, a maintainer checks the pull request's checks
list by name before merging.

## Emergency Changes

A pipeline change needed to stop an active production incident may bypass the normal
review count under the
[emergency override](../policies/APPROVAL_REQUIREMENTS.md#emergency-overrides), which
requires the `emergency` label, a written explanation, two maintainers, and the full
check suite to be run and pass afterwards. A pipeline change that relaxes a check is
never an emergency change; the check is repaired instead.

## Exceptions

Any deviation from this policy is recorded in the
[Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md) with an approver
who holds the authority to grant it and an expiry date no more than 12 months ahead,
as required by that register. A deviation that cannot be given an expiry date is
treated as a policy change instead.

## Ownership and Review

The release-management team owns this policy, with the Security Officer responsible
for the secret and permission rules. It is reviewed:

- when a workflow is added, removed, or renamed;
- when a required check changes, which happens in the same pull request as the change
  itself; and
- at least once every 12 months.

Changes to this policy are proposed in a pull request that touches only the
`Governance/` folder.

## Related Documentation

- [PR Approval Requirements](APPROVAL_REQUIREMENTS.md): approval counts and the
  required-check list this policy defers to
- [Merge Strategy Policy](MERGE_STRATEGY.md): how an approved change reaches `main`
- [EAS Build Governance](../domains/EAS_BUILD_GOVERNANCE.md): build profiles and the
  secrets used for mobile artifacts
- [Secret Rotation Policy](SECRET_ROTATION.md): cadence for the secrets referenced here
- [Bot and Automation Governance Policy](BOT_AUTOMATION.md): automation that opens pull
  requests
- [Decision Rights Matrix](../matrices/DECISION_RIGHTS_MATRIX.md): who decides when
  maintainers disagree
- [Security Officer role](../roles/SECURITY_OFFICER.md) and
  [Release Manager role](../roles/RELEASE_MANAGER.md)
- [Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md)
