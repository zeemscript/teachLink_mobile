# Data Processing Register

## Purpose

This document is the official register of data processing activities for
TeachLink Mobile. It records each processing activity's purpose, categories of
personal data, data subjects, lawful basis, retention period, processor(s),
location(s), security measures, review cadence, and status. It provides a
versioned, auditable record to support data protection compliance and
transparency.

## Scope

This register covers processing activities under the project's control in
supported deployments. It aligns with:

- [Privacy Manifest Policy](../domains/PRIVACY_MANIFEST.md) — single source of truth for data collection
- [DPA Template](../compliance/DPA_TEMPLATE.md) — agreement terms
- [Children's Data Policy](../compliance/CHILDRENS_DATA.md) — child-specific requirements
- [Mobile Telemetry Governance](../domains/MOBILE_TELEMETRY.md) — telemetry practices

## Register Table

| Processing Activity | Purpose | Data Subjects | Personal Data Categories | Lawful Basis | Retention Period | Processor(s)/Subprocessor(s) | Location(s) | Security Measures | Review Cadence | Status | Last Reviewed | Notes |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| | | | | | | | | | | | | |

*No processing activities are currently recorded. As data processing is defined
and disclosed in the privacy manifest, each distinct processing activity must
be documented here.*

## Field Definitions

- **Processing Activity**: A distinct, documented activity (e.g., user
  authentication, crash reporting, telemetry collection, content delivery).
- **Purpose**: Specific, explicit purpose for the processing. Avoid vague
  purposes like "improving service" without further specification.
- **Data Subjects**: Categories of individuals (e.g., students, teachers,
  administrators, parents/guardians). Explicitly identify child data subjects
  when applicable.
- **Personal Data Categories**: Specific categories of personal data processed
  (e.g., account identifiers, email, device identifiers, IP address, logs,
  crash reports). Must align with privacy manifest categories.
- **Lawful Basis**: The lawful basis for processing (e.g., consent, contract,
  legitimate interest, legal obligation, vital interests). Document any
  conditions or limitations. For child data, follow the Children's Data Policy.
- **Retention Period**: How long the data is retained (e.g., "30 days after
  account deletion", "90 days for crash reports"). Specify retention triggers
  and deletion confirmation process.
- **Processor(s)/Subprocessor(s)**: Who processes the data. Reference entries
  in the [Subprocessor Register](SUBPROCESSOR_REGISTER.md) where subprocessors
  are involved.
- **Location(s)**: Where data is processed (jurisdictions/regions). Note
  cross-border transfers and applicable transfer mechanisms.
- **Security Measures**: Technical and organizational measures applied (e.g.,
  encryption at rest/in transit, access controls, logging, least privilege).
  Reference the [Security Policy](../SECURITY_POLICY.md) and deployment-specific
  controls.
- **Review Cadence**: How often this processing activity is reviewed (e.g.,
  "Annually", "Quarterly", "Upon material change"). Must meet or exceed
  applicable requirements.
- **Status**: Current status of the processing activity (see definitions below).
- **Last Reviewed**: Date of most recent review (YYYY-MM-DD).
- **Notes**: Additional context, dependencies, or restrictions.

## Status Definitions

- **Active**: Currently in use and compliant with documented controls.
- **Planned**: Proposed but not yet implemented; review required before
  activation.
- **Deprecated**: Being phased out; no new processing under this activity
  without approval. Document deprecation timeline and migration plan.
- **Suspended**: Temporarily paused (e.g., due to incident or policy change).
- **Retired**: No longer active; data retention/deletion obligations documented.

## Lawful Basis and Retention

- **Lawful basis must be specified** for each processing activity. Where
  consent is the basis, consent management must follow
  [Mobile Telemetry Governance](../domains/MOBILE_TELEMETRY.md) and applicable
  child consent rules.
- **Retention periods must be concrete and enforceable.** Define what triggers
  deletion (e.g., account closure, expiration of retention window) and how
  deletion is verified/confirmed.
- **Data minimization**: Only process data necessary for the specified purpose.
  Review activities periodically to ensure minimization is maintained.

## Review Cadence

Each processing activity must have a defined review cadence. The default
review cadence is **at least annually**. However, activities involving
sensitive data, child data, or frequent changes must be reviewed more
frequently (quarterly or upon material change). The review must verify:

1. The purpose remains valid and necessary.
2. Data categories and retention periods remain appropriate (minimization).
3. Lawful basis is still valid and properly documented.
4. Subprocessors and locations remain accurate and aligned with the
   [Subprocessor Register](SUBPROCESSOR_REGISTER.md).
5. Security measures remain effective per the [Security Policy](../SECURITY_POLICY.md).
6. Compliance with the [Children's Data Policy](../compliance/CHILDRENS_DATA.md)
   where applicable.

## Update Process

1. **Alignment with manifest**: New processing activities must first be
   declared in the privacy manifest. The register entry must reflect the
   manifest exactly.
2. **Documentation**: Complete all fields. Link to related registers/policies.
3. **Review**: Changes require review by the [Security Officer](../roles/SECURITY_OFFICER.md)
   and approval per governance requirements. Where material changes affect data
   subject rights or lawful basis, additional review is required.
4. **Proposals**: Changes are proposed via a pull request touching only the
   `Governance/` folder.
5. **Record review**: When a review is completed, update "Last Reviewed" and
   add notes about the outcome (including "no change needed").

## Versioning

This register is versioned with the repository. Material changes (adding,
modifying, or retiring a processing activity; changing retention, lawful basis,
or purpose) must be documented with effective date and linked to the relevant
issue/PR and privacy manifest change.

## Review Cadence (Register Level)

The register as a whole is reviewed at least **annually**. Additionally, it is
reviewed whenever:
- The privacy manifest changes
- New subprocessors are added/removed
- Applicable law or policy changes affect processing activities
- A data protection incident occurs that reveals inaccuracies
