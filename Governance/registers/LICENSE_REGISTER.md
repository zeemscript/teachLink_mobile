# License Register

This register records the open-source licenses that TeachLink Mobile depends on
or redistributes. Its purpose is to keep the project compliant by listing each
component, its license, and where the required attribution is recorded.

## Component and License Fields

Each component records the following fields:

| Field | Description |
|-------|-------------|
| Component | Name of the library, asset, or dataset. |
| Version | Version in use. |
| Type | Dependency, asset, dataset, or bundled code. |
| SPDX ID | SPDX license identifier. |
| License name | Human-readable license name. |
| Copyright holder | Owner of the copyright. |
| Source | Upstream repository or origin URL. |
| Attribution reference | Where the attribution is recorded (`NOTICE`). |
| Compatibility | Compatibility note (see below). |
| Status | Active, Deprecated, or Removed. |

## Register

| Component | Version | SPDX ID | Copyright holder | Attribution reference | Status |
|-----------|---------|---------|------------------|-----------------------|--------|
| _none recorded_ | | | | | |

## Compatibility Note

Each component's compatibility is classified against the accepted, conditional,
and prohibited lists in
[`Governance/policies/ATTRIBUTION.md`](../policies/ATTRIBUTION.md):

- **Accepted** — on the allowlist; auto-approved.
- **Conditional** — requires review and a recorded compatibility note.
- **Prohibited** — requires an approved exception, cross-referenced in the
  [`SECURITY_EXCEPTIONS_REGISTER.md`](SECURITY_EXCEPTIONS_REGISTER.md).

The copyright holder and source must be recorded, and the attribution reference
must point to the entry in `NOTICE`.

## Review Cadence

- Reviewed **quarterly** by the Release Manager.
- New or changed components are recorded in the pull request that introduces them.
- Removed components keep their entry with a `Removed` status.

## Related Documents

- `Governance/policies/ATTRIBUTION.md`
- `Governance/registers/DEPENDENCY_REGISTER.md`
