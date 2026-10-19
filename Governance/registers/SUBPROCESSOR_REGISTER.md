# Subprocessor Register

## Purpose

This document is the official register of third-party subprocessors that may
process personal data on behalf of TeachLink Mobile in supported deployments.
It records each subprocessor's name, service provided, data categories
processed, location/jurisdiction, transfer mechanism (where applicable), and
status. It supports transparency, accountability, and compliance with data
protection requirements.

## Scope

This register applies to subprocessors used in deployments where TeachLink
Mobile acts as a processor or otherwise engages third parties to process
personal data. It is derived from the privacy manifest and deployment
configuration, not written from memory. Whether a listed subprocessor is
actually used depends on the specific deployment.

This register complements:

- [DPA Template](../compliance/DPA_TEMPLATE.md) — Annex B is derived from this list
- [Privacy Manifest Policy](../domains/PRIVACY_MANIFEST.md) — single source of truth for data collection
- [Mobile Telemetry Governance](../domains/MOBILE_TELEMETRY.md) — telemetry and observability tooling

## Register Table

| Subprocessor | Service Provided | Personal Data Categories | Location/Jurisdiction | Transfer Mechanism | Status | Effective Date | Notes |
|---|---|---|---|---|---|---|---|
| | | | | | | | |

*No subprocessors are currently declared. As third-party SDKs, services, or
observability tools are adopted and disclosed in the privacy manifest, they
must be added to this register.*

## Data Categories

When listing personal data categories, use specific, non-overlapping terms
consistent with the privacy manifest (e.g., device identifiers, IP address,
crash reports, usage metrics). Avoid generic "user data" entries. For child
data, explicitly note applicability per the [Children's Data Policy](../compliance/CHILDRENS_DATA.md).

## Location and Transfers

Record the primary processing location(s) and, if data may leave the
controller's jurisdiction, the transfer mechanism used (e.g., Standard
Contractual Clauses (SCCs), adequacy decision, Binding Corporate Rules, or other
mechanism as appropriate). If no cross-border transfer occurs for a
subprocessor in a given deployment, record "None" with explanation.

## Status Definitions

- **Active**: Currently approved for use in supported deployments; included in
  DPA Annex B when applicable.
- **Pending**: Under review prior to approval or onboarding.
- **Deprecated**: No longer approved for new use; may remain in legacy
  deployments subject to contractual obligations and change-notification rules.
- **Removed**: Removed from approved list; requires change notification as
  specified in DPA/change process.

## Change-Notification Process

Changes to this register follow the process defined in the
[DPA Template](../compliance/DPA_TEMPLATE.md):

- **General authorization**: Approved subprocessors are listed in Annex B of the
  DPA. Adding/removing subprocessors requires appropriate authorization.
- **Advance notice**: At least **30 days'** written notice must be given to
  affected controllers before adding or replacing a subprocessor (unless
  otherwise specified in the governing DPA).
- **Objection handling**: Controllers may object on reasonable data protection
  grounds. If unresolved, the controller may terminate the affected service as
  specified in the DPA.
- **Manifest consistency**: A subprocessor missing from or inconsistent with the
  privacy manifest is a defect to be fixed in both places. The source of truth
  for declared third-party services is the privacy manifest.

## Update Process

1. **Identification**: When a new third-party service that processes personal
   data is adopted, it must first be declared in the privacy manifest. Only
   then is it added to this register.
2. **Documentation**: Record all required fields, including data categories,
   location, transfer mechanism, and status.
3. **Review**: Changes require review by the [Security Officer](../roles/SECURITY_OFFICER.md)
   and appropriate governance approval per the DPA and privacy policies.
4. **Proposals**: Changes are proposed via a pull request touching only the
   `Governance/` folder.

## Versioning

This register is versioned with the repository. Each material change (add,
remove, replace, status change) must be documented with effective date and
linked to the relevant issue/PR and privacy manifest change.

## Review Cadence

This register is reviewed at least annually, and whenever the privacy manifest
changes, a new subprocessor is considered, or data transfer mechanisms change.
