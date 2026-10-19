# Deprecation Notice: [Item or capability]

Copy this template into a tracking issue or announcement and complete each
section. Link the notice from the relevant release notes and migration
documentation. Follow [`../policies/DEPRECATION.md`](../policies/DEPRECATION.md).

## Notice Details

- **Deprecation ID:** [DEP-####, from `../registers/DEPRECATION_REGISTER.md`]
- **Announced:** [YYYY-MM-DD]
- **Owner:** [Responsible maintainer or team]
- **Affected versions or platforms:** [Versions, platforms, or integrations]
- **Affected users:** [Who needs to take action]
- **Related issue, PR, or release:** [Links]

## What Is Being Deprecated

[Name and describe the feature, interface, API, or capability being deprecated.
Identify what is changing and where it is currently available.]

## Why It Is Being Deprecated

[Explain the reason for this deprecation, such as a replacement, maintenance
cost, compatibility issue, or security concern. Describe the impact of keeping
the current behavior.]

## Replacement or Alternative

- **Recommended replacement:** [Name the preferred replacement, or state that
  there is no direct replacement]
- **Alternative options:** [Other supported choices, if any]
- **Availability:** [Version or date when the replacement is available]

## Timeline

- **Notice period:** [Number of calendar days; normally at least 90]
- **Last version supporting this item:** [Version]
- **Planned removal version or date:** [Version and/or YYYY-MM-DD]
- **Shortened-period exception:** [If under 90 days, explain why waiting creates
  greater harm and identify the approver; otherwise write "Not applicable"]

## Migration Guidance

1. [Step to identify current use]
2. [Step to migrate to the replacement or alternative]
3. [Step to verify the migration]

- **Migration guide:** [Link, or state when it will be available]
- **Support contact or channel:** [Where users can get help]
- **Consequences of not migrating:** [Expected behavior after the removal date]

## Communication and Status

- [ ] Tracking issue linked from the pull request or release issue
- [ ] Release notes and `CHANGELOG.md` updated
- [ ] Relevant API, migration, or contributor documentation updated
- [ ] Affected users, support, or downstream integrators notified as applicable

- **Register status:** [Announced, Sunset, or Removed]
- **Last updated:** [YYYY-MM-DD]

## Related Documents

- [`../policies/DEPRECATION.md`](../policies/DEPRECATION.md)
- [`../registers/DEPRECATION_REGISTER.md`](../registers/DEPRECATION_REGISTER.md)
