# Privacy Policy Governance

## Purpose

This document governs the **published privacy policy**: the document users read in
the store listings and from within the app that tells them what TeachLink Mobile does
with their data. It defines who owns that policy, how it is changed and reviewed, and
how its versions are numbered, so that what users are told always matches what the
app does.

## Scope

This document covers the user-facing privacy policy and the process around it. It does
not define what the app collects, which is owned by the
[Privacy Manifest Policy](../domains/PRIVACY_MANIFEST.md) and the
[Mobile Telemetry Governance](../domains/MOBILE_TELEMETRY.md). The privacy manifest is
the single source of truth for data collection. The privacy policy and the store
privacy declarations are derived from it and must agree with it. They are not edited
independently.

It does not cover the terms of service or the accessibility statement, which have
their own governance documents.

## Ownership

| Role | Responsibility |
| --- | --- |
| [Community Manager](../roles/COMMUNITY_MANAGER.md) | **Owns** the published policy: keeps it current, publishes each version, and confirms the store links point at the latest one. The [App Store Release](../domains/APP_STORE_RELEASE.md) checklist already lists "Privacy policy current" under this role |
| [Security Officer](../roles/SECURITY_OFFICER.md) | Reviews every statement about data handling, retention, and security for technical accuracy |
| [Docs Lead](../roles/DOCS_LEAD.md) | Reviews wording for clarity and readability |
| [Maintainers](../roles/MAINTAINER.md) | Approve each new version before it is published |

The owner is accountable for the policy being accurate. A statement the owner cannot
back with the manifest or the shipped app is removed or corrected, not softened.

## What the Policy Must Cover

At minimum, and consistent with the privacy manifest:

- the categories of data collected and the purpose of each;
- whether collection is opt-in, how consent is given, and how it is withdrawn;
- which third parties and SDKs receive data, and why;
- how long data is kept and how it is deleted;
- how users exercise their rights and who to contact; and
- the effective date and version of the policy.

## When the Policy Must Change

The policy is reviewed and updated whenever any of these occurs:

- the privacy manifest changes, including a new data type, purpose, SDK, permission,
  or retention period;
- the consent flow changes;
- an app store or applicable law changes what must be disclosed;
- a privacy incident shows the policy was inaccurate or unclear; or
- the scheduled review below finds drift.

A change to what the app collects is not completable unless the policy change lands
with it or in a linked pull request, in line with the Update Process in the
[Privacy Manifest Policy](../domains/PRIVACY_MANIFEST.md).

## Update and Review Process

1. **Open a change.** A pull request or issue describes the change and links the
   privacy manifest change that prompted it.
2. **Draft.** The owner drafts the new wording in the policy source.
3. **Consistency check.** The owner and the Security Officer confirm that every claim
   matches the manifest and the shipped behaviour, and that the store privacy
   declarations will match.
4. **Review.** The Security Officer and the Docs Lead review. Where the project has
   access to legal counsel, the owner asks for review of material changes; if no
   counsel reviewed a version, the change record says so rather than implying it.
5. **Approval.** Maintainers approve the version, recording the approval in the pull
   request.
6. **Publish.** The owner publishes the new version, sets the effective date, and
   updates the links in the store listings and in the app.
7. **Verify.** Within one business day the owner confirms the live policy and the live
   store links, and records the result on the issue.

### Scheduled review

Independently of any trigger, the owner reviews the whole policy at least **every 12
months**, and before any release that changes data practices. The review confirms the
policy still matches the manifest and records the outcome, including "no change
needed".

## Versioning

The policy carries a `MAJOR.MINOR` version and an effective date.

| Change | Version bump | User notice |
| --- | --- | --- |
| A new data category, a new purpose, new sharing with a third party, a longer retention period, or a change to the consent basis | **Major** | Required, before the change takes effect |
| A clarification, a correction that does not change practice, a contact update, or a formatting change | **Minor** | Not required; recorded in the change log |

Each published version is kept and remains readable, with its effective dates, so a
user can see what applied at a given time. The store listings and the app always link
to the latest version. The policy ends with a change log:

```text
| Version | Effective | Summary of change | Change record |
| ------- | --------- | ----------------- | ------------- |
```

where the last column links the pull request or issue that approved the version.

## User Notification

For a major change, the owner arranges notice before the effective date. The notice
appears in the release notes and in the app at the next launch after the update, and
states what changed in plain language. If the change affects the basis on which the
user consented, for example a new purpose for data already collected, consent is
requested again as described in the
[Mobile Telemetry Governance](../domains/MOBILE_TELEMETRY.md). Minor changes need only
the change log entry.

## Exceptions

A legally or privacy-required correction, such as a broken policy link or a required
disclosure, may be published immediately by the Community Manager or Release Manager.
It is recorded on an issue and reviewed retroactively within one business day, as
described in the [Store Metadata Policy](../domains/STORE_METADATA.md).

## Related Documentation

- [Privacy Manifest Policy](../domains/PRIVACY_MANIFEST.md)
- [Mobile Telemetry Governance](../domains/MOBILE_TELEMETRY.md)
- [Mobile Permissions Policy](../domains/MOBILE_PERMISSIONS.md)
- [Store Metadata Policy](../domains/STORE_METADATA.md) and
  [App Store Release](../domains/APP_STORE_RELEASE.md)
- [Security Policy](../SECURITY_POLICY.md)
