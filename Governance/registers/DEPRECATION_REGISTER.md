# Deprecation Register

This register tracks deprecated features, interfaces, and capabilities in
TeachLink Mobile. Its purpose is to give users and integrators a single reference
for what is going away, the migration path, and the planned removal date.

## Deprecated-Item Fields

Each deprecated item records the following fields:

| Field | Description |
|-------|-------------|
| Deprecation ID | Sequential identifier (for example, `DEP-0001`). |
| Item | The feature, interface, or capability being deprecated. |
| Description | What the item did and why it is deprecated. |
| Replacement | The recommended replacement or migration path. |
| Announced | Date the deprecation was announced. |
| Notice period | Notice given before removal (normally at least 90 days). |
| Sunset version | First supported version without the item. |
| Removal date | Planned removal date. |
| Owner | Maintainer accountable for the deprecation. |
| Migration guide | Link to migration documentation. |
| Status | Announced, Sunset, or Removed. |

## Register

| Deprecation ID | Item | Replacement | Announced | Removal date | Status |
|----------------|------|-------------|-----------|--------------|--------|
| _none recorded_ | | | | | |

## Timeline and Replacement

Deprecations follow [`Governance/policies/DEPRECATION.md`](../policies/DEPRECATION.md).
Each entry names a replacement or migration path and a notice period that is
normally at least 90 calendar days. Shorter periods require a documented reason
and approver.

## Removal Date and Status

- The removal date is set when the deprecation is announced and does not move
  without an updated announcement.
- An item moves to `Sunset` when the replacement is available and to `Removed`
  once the removal is released.
- Removal requires that the notice period has elapsed and known consumers have
  migrated, as confirmed by the owner.

## Review Cadence

Reviewed **quarterly** by the Release Manager and the owning maintainer. Removed
items keep their entry and are never deleted.

## Related Documents

- `Governance/policies/DEPRECATION.md`
- `Governance/policies/VERSIONING.md`
