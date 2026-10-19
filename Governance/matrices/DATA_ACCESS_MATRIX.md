# Data Access Matrix

This document defines the data classifications used by TeachLink Mobile, the roles that interact with that data, and the allowed access for each role. It is the versioned reference for data access governance and is self-contained within the `Governance/` folder.

## Scope

This matrix applies to data handled by the TeachLink Mobile application, its build and release pipelines, and the supporting operational processes described in this folder. It does not grant access by itself; it defines what access is allowed for each role and must be enforced through tooling, review, and operational procedures.

## Data Classifications

Data is classified into the following categories. Classification determines the minimum controls and the maximum access any role may hold.

| Classification | Description | Examples | Handling requirements |
| --- | --- | --- | --- |
| **Public** | Information intended for open distribution. | Marketing content, public docs, open-source code. | No special handling. |
| **Internal** | Non-sensitive operational data restricted to the project team. | Internal roadmaps, build logs without PII, non-production configuration. | Access on a need-to-know basis. |
| **Confidential** | Sensitive business or technical data whose disclosure could harm the project or its users. | Private keys for signing, secrets, unreported security issues, contract terms. | Encrypted at rest, least privilege, audited access. |
| **Restricted / PII** | Personally identifiable information and other data subject to privacy obligations. | User names, email addresses, device identifiers, telemetry that can be linked to a person. | Explicit authorization, minimization, retention limits, audit trail. |

Any data that cannot be confidently classified is treated as the highest applicable classification until a decision is recorded.

## Roles

Roles are defined consistently with the rest of the governance documents in this folder.

| Role | Description |
| --- | --- |
| **Contributor** | Anyone who submits issues or pull requests to the repository. |
| **Reviewer** | Contributor trusted to review and approve changes in a designated area. |
| **Maintainer** | Responsible for merging changes, releases, and overall project health. |
| **Security team** | Handles vulnerability reports, embargoes, and advisories. |
| **Release manager** | Owns build and release pipelines and signing material. |
| **Operations / support** | Handles runtime operations and user support requests. |
| **Moderator** | Applies community and conduct policies (treated as a contributor for data access purposes). |

## Access Matrix

Legend: **F = Full** (allowed without additional approval), **R = Read-only**, **W = Write with review** (allowed with review/approval), **N = No access**.

| Data classification | Contributor | Reviewer | Maintainer | Security team | Release manager | Operations / support | Moderator |
| --- | --- | --- | --- | --- | --- | --- | --- |
| **Public** | F | F | F | F | F | F | F |
| **Internal** | R | F | F | R | F | R | R |
| **Confidential** | N | N | W | F | W | N | N |
| **Restricted / PII** | N | N | W | W | N | R | N |

### Notes on the matrix

- **Full** access is still subject to the handling requirements for the classification.
- **Write with review** means the change must be approved by a second person holding at least the same access for that classification.
- **Security team** access to Restricted / PII data is limited to incident response and is subject to the disclosure process in this folder.
- **Operations / support** access to Restricted / PII data is read-only and must be justified by an open support request.
- No role has access to data outside the scope defined above; any exception requires a decision record in this folder.

## Review Cadence

- This matrix is reviewed at least **every six months** by the maintainers and security team.
- An **ad-hoc review** is triggered by any of the following:
  - a new data classification is introduced;
  - a new role is added or an existing role's responsibilities change;
  - a security incident or privacy complaint is resolved;
  - a relevant regulatory or platform requirement changes.
- Review outcomes are recorded by updating this document in a pull request that touches only the `Governance/` folder.
- If no changes are needed, the review is recorded in the relevant issue or pull request history.

## Change Log

| Date | Change | Author |
| --- | --- | --- |
| 2025-01-01 | Initial data access matrix for TeachLink Mobile. | Governance }
