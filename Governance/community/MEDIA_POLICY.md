# Press and Media Policy

## Purpose

This policy defines who speaks to journalists, bloggers, podcasters, and other media
on behalf of TeachLink Mobile, how a statement is approved, and what it may say. It
exists so that what the public hears about the project is accurate, consistent, and
does not expose users, contributors, or unreleased security information.

## Scope

This policy covers interviews, press releases, statements, quotes, comments to
journalists, and responses to media enquiries about the project, its releases, its
people, or its governance. It applies to maintainers, role holders, and any
contributor who is asked to speak for the project.

It does not stop anyone speaking about their own experience or opinions in a personal
capacity (see [Personal Statements](#personal-statements)). Speaking at events and
conferences is covered by the [Representation Policy](REPRESENTATION.md). Security
disclosure is governed by the [Security Embargo Policy](../policies/EMBARGO.md) and
the [Vulnerability Disclosure process](../processes/VULN_DISCLOSURE.md), which take
precedence for security matters.

## Who Speaks for the Project

| Topic | Spokesperson | Notes |
| --- | --- | --- |
| General project news, community, events, and contributions | [Community Manager](../roles/COMMUNITY_MANAGER.md) | The default spokesperson |
| A release and its contents | [Release Manager](../roles/RELEASE_MANAGER.md), with the Community Manager | Coordinates timing with the release announcement |
| Technical direction and architecture | A [Maintainer](../roles/MAINTAINER.md) designated for the enquiry | |
| Security incidents and vulnerabilities | [Security Officer](../roles/SECURITY_OFFICER.md) | Only after the embargo allows it |
| Governance disputes, conduct matters, or legal questions | No one speaks. See [Matters We Do Not Comment On](#matters-we-do-not-comment-on) | |

Everyone else may refer an enquiry to the Community Manager. They need not say more.
A maintainer may designate another person as a spokesperson for a specific enquiry; the
designation is written, names the topic, and expires when the enquiry closes.

## Handling an Enquiry

1. **Receive.** Anyone who is contacted by the media forwards the enquiry to the
   Community Manager without answering substantively, and says they will pass it on.
2. **Log.** The Community Manager records the outlet, the contact, the topic, and the
   deadline in a restricted issue.
3. **Triage.** The Community Manager decides who should respond, using the table
   above, and acknowledges the enquiry within **three business days**.
4. **Draft.** The designated spokesperson drafts the response in writing.
5. **Approve.** The statement is approved according to the next section.
6. **Respond and record.** The response is sent, and the final text is stored with the
   enquiry so it can be checked later.

## Approval

| Type of statement | Approval required |
| --- | --- |
| Factual statement about already-released features or published documents | Community Manager |
| Statement about the roadmap, plans, or anything not yet released | Community Manager and one Maintainer |
| Quote attributed to a named person other than the speaker | That person's consent, plus the Community Manager |
| Statement about a security issue | Security Officer and two Maintainers, within the embargo rules |
| Statement about a legal, regulatory, or governance dispute | Maintainer consensus; normally the answer is no comment |

Approval is recorded in the enquiry issue. If a deadline cannot be met with the
required approvals, the project may decline to comment; it does not skip approval to
meet a deadline.

## Messaging Guidance

Statements should be:

- **Accurate.** Say only what is true of the released app and the published
  governance. Link to the source where one exists.
- **Plain.** Use language a teacher or student can follow, and avoid jargon.
- **Consistent with the mission.** Describe the project in the terms of the
  [Charter](../CHARTER.md) and [Vision](../VISION.md).
- **Modest about the future.** Do not promise dates, features, or outcomes that are
  not committed in the roadmap.
- **Respectful.** Do not disparage individuals, other projects, or competitors.

Do not state or imply:

- that the project is endorsed by, or partnered with, an organisation without written
  agreement (see the [Trademark Policy](../policies/TRADEMARK.md));
- anything about an unreleased vulnerability, or any detail covered by an embargo;
- any user's data or identity, or any private information about a contributor; or
- the outcome or details of a conduct case, which are confidential under the
  [conduct reporting](../COC_REPORTING.md) and [enforcement](../COC_ENFORCEMENT.md)
  documents.

## Matters We Do Not Comment On

The project does not comment on: active security vulnerabilities before disclosure;
individual conduct cases or moderation decisions; legal matters; and rumours or
speculation. The standard response is: "We don't comment on that. Our published
governance describes how the project handles it," with a link where one applies.

## Corrections

If the project's name is attached to an inaccurate statement, the Community Manager may
ask the outlet for a correction. A correction that the project itself needs to make is
published promptly, states what was wrong, and is recorded in the enquiry issue.

## Personal Statements

Contributors and role holders may speak in their own name. When they do, they:

- make clear they speak for themselves, for example "my own view, not the project's";
- do not use a project title to lend weight to a personal opinion or a commercial
  interest; and
- do not disclose confidential information obtained through a role.

## Records

The enquiry issue is the record: who asked, who was designated, the approvals, and the
final text. Restricted issues are visible to maintainers and the Community Manager.
The Community Manager includes a summary of media activity in the monthly community
health report.

## Related Documentation

- [Representation Policy](REPRESENTATION.md)
- [Trademark and Brand Usage Policy](../policies/TRADEMARK.md)
- [Security Embargo Policy](../policies/EMBARGO.md)
- [Community Manager role](../roles/COMMUNITY_MANAGER.md)
