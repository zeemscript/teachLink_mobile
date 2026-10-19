# Dependency Register

This register tracks the third-party dependencies TeachLink Mobile relies on. Its
purpose is to give maintainers one versioned reference for what is used, who owns
it, and under which license, so reviews and security audits can reason about the
supply chain.

## Tracked Fields

Each dependency records the following fields:

| Field | Description |
|-------|-------------|
| Name | Package or component name as declared in the manifest. |
| Version | Version constraint or resolved version in use. |
| Type | Direct or transitive dependency. |
| Ecosystem | Package manager (npm, CocoaPods, Maven, Pub, other). |
| Purpose | Why the dependency is used. |
| Owner | Maintainer or role accountable for keeping the dependency current. |
| License | SPDX identifier for the dependency license. |
| Attribution | Where the required attribution is recorded (`NOTICE`). |
| Added | Date the dependency was introduced. |
| Last reviewed | Date the entry was last verified. |
| Status | Active, Deprecated, or Removed. |

## Register

| Name | Version | Type | Ecosystem | Owner | License | Status |
|------|---------|------|-----------|-------|---------|--------|
| _none recorded_ | | | | | | |

## Ownership and License

Every dependency must name an owner before it is merged. The owner confirms the
license is on the accepted list in
[`Governance/policies/ATTRIBUTION.md`](../policies/ATTRIBUTION.md). Dependencies
under a prohibited or conditional license require an approved exception and are
cross-referenced in the
[`SECURITY_EXCEPTIONS_REGISTER.md`](SECURITY_EXCEPTIONS_REGISTER.md).

## Review Cadence

- Reviewed **quarterly** by the maintainers with the Security Officer.
- Any new dependency is added to the register in the pull request that introduces it.
- The register is append-only; removed dependencies keep their entry with a
  `Removed` status and the removal date.

## Related Documents

- `Governance/policies/ATTRIBUTION.md`
- `Governance/registers/LICENSE_REGISTER.md`
