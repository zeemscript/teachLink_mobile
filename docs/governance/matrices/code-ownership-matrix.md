# Code Ownership Matrix

This matrix maps areas of the TeachLink Mobile codebase to an owning role and
defines the review requirement for changes in each area. It makes the reviewer
for any given change unambiguous and keeps review load predictable.

## Areas of the Codebase

| Area | Path | Owning Role | Review Requirement |
|------|------|-------------|--------------------|
| App screens & navigation | `app/` | Mobile maintainer | 1 reviewer |
| Shared components | `components/` | Mobile maintainer | 1 reviewer |
| Hooks & state | `hooks/`, `src/` | Mobile maintainer | 1 reviewer |
| Platform config | `app.config.ts`, `eas.json`, `ios/`, `android/` | Release Manager | 1 maintainer |
| CI/CD & tooling | `.github/`, `scripts/`, `tools/` | Maintainer | 1 maintainer |
| Security & secrets | `plugins/`, signing config | Security Officer | 1 maintainer + Security Officer |
| Governance docs | `Governance/`, `docs/governance/` | Docs Lead | 1 maintainer |
| Tests | `tests/`, `__tests__/` | Area owner | 1 reviewer |

## Review Rules

- The owning role is the default reviewer; any maintainer may review if the owner
  is unavailable.
- Changes that span multiple areas require an approval from each area owner.
- Security-sensitive paths always require the Security Officer in addition to the
  area owner.
- The owning role is accountable for keeping its area's documentation current.

## Escalation

Disagreement between an author and an area owner is escalated using
`Governance/processes/CONFLICT_RESOLUTION.md`.

## Review Cadence

Reviewed **bi-annually** by the maintainers, and whenever a path is added,
renamed, or removed.
