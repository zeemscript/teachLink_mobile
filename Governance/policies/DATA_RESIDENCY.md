# Data Residency Policy

## Purpose

This policy defines where TeachLink Mobile's data may be stored, which transfers are
permitted, and how a request to store it elsewhere is approved. Residency decisions are
made once and are hard to reverse: moving a store, changing a backup region, or adding a
processor in another jurisdiction commits the project to that choice, and data about
children carries consequences beyond the engineering.

## Scope

This policy covers the storage location of every category of data the app processes,
including:

- account and authentication data, such as tokens and profiles;
- student, teacher, and school records;
- lesson content and offline cache data, per the
  [Offline Data Governance](../domains/OFFLINE_DATA.md);
- telemetry, crash reports, and analytics events;
- push notification tokens and message content;
- in-app purchase and billing records held by the stores;
- backups, replicas, and disaster-recovery copies; and
- data processed by third parties on the project's behalf, including the AI and
  analytics services in the [Subprocessor Register](../registers/SUBPROCESSOR_REGISTER.md).

It does not define what data is collected or on what basis, which is the
[Privacy Manifest Policy](../domains/PRIVACY_MANIFEST.md) and the
[Consent Management Policy](CONSENT_MANAGEMENT.md); how long data is kept, which is the
[Record Retention and Legal Hold Policy](RECORD_RETENTION.md); or the terms with a
processor, which are the [DPA Template](../compliance/DPA_TEMPLATE.md).

## Principle

Personal data is stored and processed in its **declared primary region**, and is not
transferred outside that region except where this policy permits it and the
[Data Processing Register](../registers/DATA_PROCESSING_REGISTER.md) records it.

The region is declared, not implied. A system with no stated region is treated as having
no approved region, and the default answer is that new storage in it is not approved
until the register says otherwise.

## Storage by Data Category

The table records where each category may be stored. Where the project operates no
infrastructure of its own, the region is the one operated by the named provider, which
is why the provider is named rather than a region label alone.

| Data category | Primary storage | Region | Notes |
| --- | --- | --- | --- |
| Account and authentication data | Primary application database | As declared in the [Data Processing Register](../registers/DATA_PROCESSING_REGISTER.md) | Backups in the same region |
| Student, teacher, and school records | Primary application database | Same region as account data | Not replicated across regions |
| Lesson content and media | Object storage | Same region as the database | Replicated within region only |
| Push notification tokens | Push delivery provider | Provider's region, as declared in the [Subprocessor Register](../registers/SUBPROCESSOR_REGISTER.md) | Tokens only; message content is not retained by the push provider beyond delivery |
| Crash reports and diagnostics | Crash reporting provider | Provider's region | Must not contain personal data, per the [Mobile Telemetry Governance](../domains/MOBILE_TELEMETRY.md) |
| Product analytics | Analytics provider | Provider's region | Pseudonymised identifiers only |
| Backups and disaster recovery | Backup storage | Same region as the primary store | Backups are not a loophole: a backup may not be used to relocate data |
| Offline cache on device | User's device | The user's device | Deleted with the app or by the user; see the [Right-to-Erasure Policy](DATA_DELETION.md) |
| Store billing records | App Store and Play Store | The store's region | Held by the platform; the project receives limited metadata |

Two rules hold for every row:

- **A category's primary store and its backups, replicas, and disaster-recovery copies
  stay in the same region.** Separating them for durability does not change residency.
- **Where the project does not operate the infrastructure, the provider's region is the
  project's region for that category**, and a provider changing regions is a
  [subprocessor change](../registers/SUBPROCESSOR_REGISTER.md#change-notification-process)
  requiring notice before it takes effect.

## Transfer Restrictions

A cross-border transfer is restricted, and each transfer must be justified in one of
three ways.

| Transfer type | Permitted | Conditions |
| --- | --- | --- |
| Data processed by a processor located outside the region | **Yes, with conditions** | The processor is in the [Subprocessor Register](../registers/SUBPROCESSOR_REGISTER.md); a DPA is in place per the [DPA Template](../compliance/DPA_TEMPLATE.md); the transfer mechanism is recorded; and users are told in the privacy policy |
| Data replicated to a backup or DR region outside the primary | **Only by exception** | Approved under the exception process below, with the region, the volume, and the duration recorded, and a plan to return it |
| Data transferred to a country the project's legal basis cannot reach | **No** | Not permitted by any exception. Where a transfer is legally prohibited, the feature or the provider is replaced rather than the transfer approved |
| Data moved between the project's own regions for availability | **Only with notice and a stated purpose** | Recorded in the register, and the purpose stated to users where it affects them |

Rules that apply to every transfer:

- **Transfer is a decision, not a side effect.** A provider that replicates data
  globally by default is a transfer, and requires the same approval as a deliberate one.
- **The transfer mechanism is recorded.** The mechanism used for each transfer — such as
  standard contractual clauses — is named in the register, so a change in mechanism is
  visible.
- **Telemetry is not exempt.** Crash reports and analytics that leave the region are
  transfers. They are acceptable only because they carry no personal data, which is a
  condition of the [Mobile Telemetry Governance](../domains/MOBILE_TELEMETRY.md) rather
  than a waiver of this policy.
- **Data on a user's device is under that user's control** and is not a residency
  question for the project, though its handling is governed by the
  [Encryption Policy](ENCRYPTION.md).

## Exception Process

An exception is required for any new storage location, region, or transfer not already
recorded as permitted above.

1. **Open a request.** The request states the region, the data categories involved, the
   provider, and the business reason the approved location is unavailable.
2. **Assess the risk.** The request describes what the change means for the people
   whose data is involved, with particular attention to the
   [Children's Data Policy](../compliance/CHILDRENS_DATA.md), since residency risk
   differs sharply for data about children.
3. **Name the controls.** Every request names compensating controls: encryption in
   transit and at rest per the [Encryption Policy](ENCRYPTION.md), restricted processor
   access, a defined deletion path, and whether the transfer is visible to users in the
   privacy policy.
4. **Propose an expiry.** An exception has an expiry date, no more than 12 months ahead.
   A request without one is refused rather than granted conditionally.
5. **Approve.** The [Security Officer](../roles/SECURITY_OFFICER.md) and a maintainer
   approve, since residency affects both security and the published privacy position.
   Where the project has access to legal counsel, the request is put to them, and if
   they did not review it the record says so rather than implying they did.
6. **Record.** The exception is entered in the
   [Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md), and the
   affected rows are added to the [Data Processing Register](../registers/DATA_PROCESSING_REGISTER.md)
   with the region and the transfer mechanism.
7. **Tell users.** Where the exception changes what the privacy policy says, the policy
   change is published before the change takes effect, following the
   [Privacy Policy Governance](../compliance/PRIVACY_POLICY_GOVERNANCE.md). A residency
   change is never internal-only.
8. **Review before expiry.** The approver is reminded ahead of the expiry date, and the
   exception is closed or re-approved with fresh justification.

Where the project's legal obligations in a given jurisdiction make the approved location
unavailable, the answer is to change the feature or the provider, not to store the data
anyway and record an exception.

## Enforcement Status

- Residency is a property of infrastructure configuration, not of application code, and
  the project operates no database itself. This policy therefore governs the provider
  choices recorded in the [Data Processing Register](../registers/DATA_PROCESSING_REGISTER.md)
  and the [Subprocessor Register](../registers/SUBPROCESSOR_REGISTER.md).
- No automated check verifies that a provider's region has not changed. Provider
  region changes are caught through the subprocessor review, which is a human activity
  on a stated cadence.
- Where a future backend is operated by the project, a region check in deployment
  configuration would be the natural enforcement point, introduced under the
  [Bot and Automation Governance Policy](BOT_AUTOMATION.md).

## Exceptions

Any deviation from the storage table or the transfer restrictions is recorded in the
[Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md) through the process
above, with an approver holding the authority to grant it and an expiry no more than 12
months ahead.

## Ownership and Review

The [Security Officer](../roles/SECURITY_OFFICER.md) owns this policy with the
[Release Manager](../roles/RELEASE_MANAGER.md), and the
[Community Manager](../roles/COMMUNITY_MANAGER.md) owns what the privacy policy says
about it. It is reviewed whenever a subprocessor or a provider changes, when a new data
category is added, when a legal requirement changes, and at least once every 12 months.
Changes are proposed in a pull request that touches only the `Governance/` folder.

## Related Documentation

- [Data Processing Register](../registers/DATA_PROCESSING_REGISTER.md): category,
  region, and transfer mechanism
- [Subprocessor Register](../registers/SUBPROCESSOR_REGISTER.md): every third party and
  its change-notification period
- [DPA Template](../compliance/DPA_TEMPLATE.md): processor terms
- [Privacy Manifest Policy](../domains/PRIVACY_MANIFEST.md): what is collected
- [Privacy Policy Governance](../compliance/PRIVACY_POLICY_GOVERNANCE.md): what users
  are told
- [Record Retention and Legal Hold Policy](RECORD_RETENTION.md): how long data is kept
- [Right-to-Erasure Policy](DATA_DELETION.md): how a user's data is deleted
- [Encryption Policy](ENCRYPTION.md): protection in transit and at rest
- [Children's Data Policy](../compliance/CHILDRENS_DATA.md): the heightened standard
- [Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md)
