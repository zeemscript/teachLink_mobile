# Support Policy

## Purpose

This policy defines what support TeachLink Mobile offers, through which channels,
within what response expectations, and what is explicitly out of scope. Without a
written scope, every unanswered question becomes an implicit promise, and the
difference between "we will look at it" and "we will fix it" is decided by whoever
happens to read the issue.

## Scope

This policy covers support **requests** — questions and reports that arrive through a
support channel and need a response. It does not cover the underlying engineering
processes, which are governed elsewhere:

| Concern | Owner |
| --- | --- |
| Security vulnerability reports | [Security Policy](../SECURITY_POLICY.md) |
| Code of Conduct concerns | [Code of Conduct Reporting](../COC_REPORTING.md) |
| Review turnaround on pull requests | [Code Review Service Level Agreement](REVIEW_SLA.md) |
| Security remediation deadlines | [Vulnerability Remediation SLA Policy](VULN_SLA.md) |
| Defect triage and prioritisation | [Issue Triage Process](../processes/TRIAGE.md) |
| Data subject requests, such as erasure | [Right-to-Erasure Policy](DATA_DELETION.md) |

Where a request arrives in the wrong channel, it is redirected to the right one rather
than answered informally in the wrong place. A vulnerability report sent to a public
issue is treated as disclosed and is handled under the
[Security Policy](../SECURITY_POLICY.md) immediately, not after a public comment.

## Support Channels

| Channel | Use it for | Response expectation |
| --- | --- | --- |
| GitHub issues | Defect reports, feature requests, and questions that benefit from a public, searchable answer | Acknowledged within **5 business days**; see [Response Expectations](#response-expectations) |
| GitHub discussions | Open-ended questions, ideas, and use cases that are not defects | Acknowledged within **7 business days** |
| Security reporting channel | Anything security-sensitive, including a vulnerability in the app or the backend | Per the [Security Severity Rubric](../SECURITY_SEVERITY_RUBRIC.md#response-sla-per-level) |
| Code of Conduct reporting | Behaviour or conduct concerns | Per the [Code of Conduct Reporting](../COC_REPORTING.md) process |
| Documentation | Usage and contribution questions | No response; the documentation is the answer |

Rules that apply to every channel:

- **One question per issue.** A broad issue asking several unrelated things cannot be
  tracked to closure. It is split, or answered with a pointer to the individual issues.
- **Search first, ask second.** The requester is asked to search existing issues, and
  a duplicate is closed with a link to the original rather than answered twice.
- **Answer in public by default.** Anything about the app's behaviour is answered in the
  issue, so the next person finds it. Personal data is not, and a request containing
  personal data is redirected to a private channel before it is discussed.
- **Acknowledgement is not resolution.** Acknowledging a request promptly and then
  leaving it silent for months is the failure mode this policy is designed to prevent.
- **No support by private message.** Maintainers are asked to redirect private requests
  to a public channel, which protects both the requester and the maintainer.

## Response Expectations

Response expectations are stated in **business days**, Monday to Friday, excluding
public holidays in the project's primary time zone.

| Request type | Acknowledgement | First substantive response | Update cadence if unresolved |
| --- | --- | --- | --- |
| Defect with a clear reproduction | 2 business days | 5 business days | Every 10 business days |
| Defect that cannot be reproduced from the description | 3 business days | Ask for the missing detail within 5 business days | Every 10 business days |
| Feature request | 5 business days | Triaged within 10 business days; no commitment to implement | Every 30 days until triaged |
| Question about behaviour or documentation | 3 business days | 5 business days | Every 10 business days |
| Security report | Per severity rubric | Per severity rubric | Per severity rubric |
| Code of Conduct report | Per the enforcement policy | Per the enforcement policy | Per the enforcement policy |
| Contributor blocked on an access or process question | 5 business days | 10 business days | Every 10 business days |

What "acknowledgement" and "first substantive response" mean:

- **Acknowledgement** confirms the request was seen, is in the right channel, and is
  not already covered elsewhere. It does not promise a fix or a date.
- **First substantive response** gives the requester something actionable: triage
  outcome and severity, the missing detail needed to proceed, a link to an existing
  issue, or a decision not to pursue with the reason.
- **An update cadence is a promise to stay in contact.** A request that goes quiet is
  worse than one that is declined, because the requester cannot plan around it.

Nothing in this policy promises a fix. Whether a defect is fixed, and when, follows the
[Issue Triage Process](../processes/TRIAGE.md) and the
[Milestone Governance](../processes/MILESTONE_GOVERNANCE.md), and the answer is
communicated as soon as it exists.

## Out of Scope

The following are not support obligations, and saying so early is kinder than
disappointing someone later.

| Out of scope | Why, and what to do instead |
| --- | --- |
| Guaranteeing a fix, a date, or inclusion in a release | Work is prioritised through triage and milestone governance. Ask for it as a feature request, which is tracked |
| Support for an end-of-life release line or OS version | Out of the support window per the [Compatibility and Support Window Policy](COMPATIBILITY.md). Update to a supported version |
| Support for modified, repackaged, or unofficial builds | Not produced or verified by the project, and signing is covered by the [App Signing Key Policy](../domains/APP_SIGNING_KEYS.md) |
| Third-party services, hardware, or other apps | Point the requester to that provider; where it affects the app, a defect report with the integration details is welcome |
| Customisation, white-labeling, or feature development for one organisation | A request to a maintainer is not free capacity; see the project's scope in the [Scope Statement](../SCOPE.md) |
| Deployment, hosting, or backend operations for a self-hosted instance | Not something the project operates or supports; this is stated rather than discovered per request |
| Data recovery for content lost on a device the user no longer controls | Explained with the reason, per the [Offline Data Governance](../domains/OFFLINE_DATA.md) |
| Legal advice, and interpretation of data-protection law for a specific case | The project can point to the published [Privacy Policy Governance](../compliance/PRIVACY_POLICY_GOVERNANCE.md), not advise on a case |
| Assessment of a third party's security posture | Not in scope; reports about a dependency are welcome as a defect, per the [Vendor Risk Policy](VENDOR_RISK.md) |
| Feature requests that conflict with the [Non-Goals](../NON_GOALS.md) or the [Vision](../VISION.md) | Declined with a link to those documents, which is a decision rather than a deferral |

An out-of-scope request is answered politely, with the reason and, where one exists, a
pointer to the right place. It is not dismissed without an answer.

## Escalation

A request that is not answered within its response expectation, or that the requester
believes was mishandled, escalates through the
[Escalation Path](../processes/ESCALATION_PATH.md). An unanswered accessibility barrier,
a request involving children's data, or a request from someone blocked by a language or
disability barrier escalates immediately rather than waiting for the full expectation
to lapse.

Escalation of a security concern skips this policy entirely and goes straight to the
[Security Policy](../SECURITY_POLICY.md).

## Data in Support Requests

- A support request must not contain personal data about a student or child. If it
  does, the requester is asked to remove it and it is not quoted in a public reply.
  Concerns about children's data follow the
  [Children's Data Policy](../compliance/CHILDRENS_DATA.md).
- A requester is asked not to include passwords, tokens, or keys, and any that arrive
  are treated as exposed and rotated per the
  [Secret Rotation Policy](SECRET_ROTATION.md).
- Logs or screenshots are shared through the channels the [Security Policy](../SECURITY_POLICY.md)
  describes, not pasted into a public issue, when they may contain personal data.

## Enforcement Status

- Response expectations are a human commitment tracked by the
  [Issue Triage Process](../processes/TRIAGE.md) and reviewed in the maintainer
  review. No automation measures response times or warns about an ageing issue.
- The first-response timings are consistent with the
  [Stale Issue Policy](STALE_ISSUES.md), which handles issues that receive no
  maintainer attention.
- Enforcement is therefore by review and by the monthly report, not by a gate.

## Ownership and Review

The maintainers and the [Community Manager](../roles/COMMUNITY_MANAGER.md) own this
policy, with the [Issue Triager](../roles/TRIAGER.md) responsible for the triage
response. It is reviewed when a channel is added or removed, when the support model
changes, and at least once every 12 months. Changes are proposed in a pull request that
touches only the `Governance/` folder.

## Related Documentation

- [Issue Triage Process](../processes/TRIAGE.md): how a defect is prioritised
- [Code Review Service Level Agreement](REVIEW_SLA.md): reviewer and author
  expectations on pull requests
- [Escalation Path](../processes/ESCALATION_PATH.md): what to do when a response
  expectation is missed
- [Stale Issue Policy](STALE_ISSUES.md) and [Stale Pull Request Policy](STALE_PRS.md)
- [Security Policy](../SECURITY_POLICY.md) and
  [Code of Conduct Reporting](../COC_REPORTING.md)
- [Compatibility and Support Window Policy](COMPATIBILITY.md) and
  [End-of-Life Policy](END_OF_LIFE.md)
- [Contributor Onboarding Process](../processes/ONBOARDING.md) and
  [Community Manager role](../roles/COMMUNITY_MANAGER.md)
