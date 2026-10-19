# Contributor Agreement Overview

## Purpose

This document summarises, in one place, what a person agrees to when they contribute
to TeachLink Mobile. It is an overview for contributors and for the maintainers who
review their work. It points to the policies that define each commitment in full and
does not replace them.

## Scope

This overview applies to anyone who submits a change to this repository, including
code, documentation, tests, translations, and `Governance/` changes, whether as an
individual, as an employee, or on behalf of an organisation.

The project does **not** ask contributors to sign a separate Contributor License
Agreement (CLA). There is no signature service, account, or form. Contributors make
their commitments through the sign-off on each commit, which is why most of this
document is about that mechanism.

## What Contributors Agree To

| Commitment | What it means | Defined in |
| --- | --- | --- |
| **Right to contribute** | The contributor wrote the change or has the right to submit it under the project's terms, and has not copied work they are not entitled to share | [DCO Policy](../policies/DCO.md) |
| **Public and permanent record** | The contribution and the personal information submitted with it, including the sign-off, are public and are kept indefinitely | [DCO Policy](../policies/DCO.md), clause (d) |
| **Licensing of the contribution** | The contribution is submitted under the licence that applies to the file or project at the time it is made | See [Open items](#open-items) |
| **Conduct** | Contributors follow the project's conduct expectations and accept the enforcement process | [Code of Conduct Enforcement](../COC_ENFORCEMENT.md), [reporting](../COC_REPORTING.md) |
| **Security disclosure** | Vulnerabilities are reported privately and not disclosed publicly before a fix | [Security Policy](../SECURITY_POLICY.md), [Vulnerability Disclosure](../processes/VULN_DISCLOSURE.md) |
| **Review and approval rules** | A change is merged only after the required review and checks | [Code Review Policy](../policies/REVIEW_POLICY.md), [PR Approval Requirements](../policies/APPROVAL_REQUIREMENTS.md) |
| **Attribution** | Third-party material is credited as required | [Attribution Policy](../policies/ATTRIBUTION.md) |

A contributor who is employed, or who contributes in the course of other work, is
responsible for confirming that their employer or client permits the contribution.
The sign-off is the contributor's own statement; the project cannot verify it.

## Sign-Off Mechanism

Every commit in a pull request carries a sign-off line:

```text
Signed-off-by: Name <email>
```

Git adds it when a commit is made with the `-s` flag:

```bash
git commit -s -m "fix(courses): handle empty enrolment list"
```

Rules, summarised from the [Contributor and Release Sign-off Policy](../policies/CONTRIBUTOR_SIGNOFF.md)
and the [DCO Policy](../policies/DCO.md):

- The name and email in the line match the commit author, so a sign-off cannot be
  used to assert authorship for someone else's work.
- A commit without a sign-off is not merged. Rewriting a branch (rebase, squash)
  requires the lines to be present on the rewritten commits.
- The sign-off certifies the right to contribute. It is **not** a cryptographic
  signature; any signing requirement for maintainers is a separate matter and is
  governed by its own policy.

### Current enforcement

The DCO Policy records that automated verification of the sign-off is not yet in the
CI workflows, and that maintainers verify it by hand. This overview does not repeat
that analysis; read the "Follow-up obligation" in the [DCO Policy](../policies/DCO.md)
for the current state. Until the automated check exists, a missing sign-off is caught
at review.

## Where Records Are Kept

| Record | Where it lives | Notes |
| --- | --- | --- |
| The sign-off | In each commit, in the repository history | The record of record. No separate private log is kept |
| Review and approval | The pull request thread | Includes who approved and any deferred comments |
| Decisions about the agreement itself | The issue or pull request that changed this document | Linked from the commit that changed it |
| Conduct reports and outcomes | The restricted conduct record | Handled privately under the [reporting](../COC_REPORTING.md) and [enforcement](../COC_ENFORCEMENT.md) documents |

Because the history is public and permanent, a contributor who later stops agreeing
with the terms can stop signing future commits. Nothing edits the commits they have
already made.

## Changing This Overview or the Terms

- This overview is a `Governance/` document. It changes through a pull request that
  touches only that folder, with the usual review.
- A change that alters what contributors commit to (for example, replacing the DCO
  with another mechanism) is a change to the underlying policy first. It follows the
  [RFC process](../processes/RFC_PROCESS.md), and this overview is updated to match.
- A change applies to contributions made after it takes effect. It does not
  retroactively change commits already merged.

## Open items

| Item | Detail | Owner |
| --- | --- | --- |
| Licence statement | The repository root has no `LICENSE` file, and `package.json` declares no licence. The licence a contribution is submitted under should be stated explicitly where contributors will see it. This overview deliberately names no licence rather than guessing one | Maintainers |
| Automated sign-off check | Tracked in the DCO Policy follow-up | Security Officer and maintainers |

## Related Documentation

- [DCO Policy](../policies/DCO.md) and [Contributor and Release Sign-off Policy](../policies/CONTRIBUTOR_SIGNOFF.md)
- [Contributor role](../roles/CONTRIBUTOR.md)
- [Governance README](../README.md)
