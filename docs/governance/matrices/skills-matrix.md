# Skills Matrix

This matrix records the skill coverage across the maintainer team and highlights
the gaps the project needs to close. Its purpose is to keep the project
sustainable by making sure every critical area has both a primary and a backup
owner.

## Skill Areas

| Skill Area | Description |
|------------|-------------|
| Mobile app (React Native / Expo) | Screens, navigation, state, native modules. |
| iOS platform | Xcode, provisioning, App Store submission. |
| Android platform | Gradle, signing, Play Store submission. |
| Security | Vulnerability triage, secrets, disclosure. |
| CI/CD & build | Pipelines, EAS builds, release automation. |
| Accessibility | WCAG 2.1 AA conformance and audits. |
| Data & privacy | FERPA/COPPA/GDPR, retention, consent. |
| Documentation | Governance and developer documentation. |

## Coverage per Maintainer

Coverage reflects the maintainers who can review and unblock work in each area.

| Skill Area | Primary | Backup | Coverage |
|------------|---------|--------|----------|
| Mobile app (React Native / Expo) | Maintainer A | Maintainer B | Strong |
| iOS platform | Maintainer A | Maintainer C | Adequate |
| Android platform | Maintainer B | Maintainer C | Adequate |
| Security | Security Officer | Maintainer A | Strong |
| CI/CD & build | Maintainer C | Maintainer A | Adequate |
| Accessibility | Maintainer B | Maintainer D | Limited |
| Data & privacy | Security Officer | Maintainer D | Limited |
| Documentation | Docs Lead | Maintainer B | Strong |

## Gaps and Plan

| Gap | Risk | Plan |
|-----|------|------|
| Accessibility coverage is limited | Reduced conformance to the accessibility merge gate | Recruit and mentor a second accessibility reviewer. |
| Data & privacy coverage is limited | Slower privacy reviews for releases | Pair the Security Officer with a maintainer and document the checklist. |
| Single primary on iOS | Release stall if the primary is unavailable | Cross-train the backup on signing and submission. |

## Review Cadence

Reviewed **bi-annually** by the maintainers and updated whenever a maintainer
joins, leaves, or changes focus. Gaps are tracked as issues and revisited at the
governance review.
