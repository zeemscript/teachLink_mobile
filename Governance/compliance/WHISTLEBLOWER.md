# Whistleblower Policy

## Purpose

This policy defines how someone who observes wrongdoing in TeachLink Mobile can report
it, what protection they have if they do, and what the project does with the report. It
exists so that a person with a genuine concern has a safe route to raise it, and so that
the project has a defined process rather than improvising when one is used.

## Scope

This policy covers reports of suspected wrongdoing connected with the project: conduct
breaches, misuse of access or credentials, financial or procurement irregularity,
fraud, harassment, discrimination, safety concerns, and attempts to conceal any of these.

It does not replace the processes that already exist for specific categories, and a
report is routed to the right one:

| Report about | Primary process |
| --- | --- |
| A vulnerability in the software | [Security Policy](../SECURITY_POLICY.md), private channel |
| Code of Conduct or conduct at the project | [Code of Conduct Reporting](../COC_REPORTING.md) |
| A data protection breach or a failed erasure request | [Security Policy](../SECURITY_POLICY.md), handled as an incident |
| A governance or decision dispute | [Conflict Resolution Process](../processes/CONFLICT_RESOLUTION.md) |
| Retaliation against a reporter | This policy, treated as the most serious category |

This policy is the **safeguard** for those processes: it covers the situations where a
person cannot safely use them, cannot reach the right person, or is afraid of the
consequences of doing so.

## Reporting Channels

A reporter chooses the channel. More than one is offered because the right channel
depends on who the reporter trusts, and a single named channel is not usable by everyone.

| Channel | Best for | Notes |
| --- | --- | --- |
| Email to the [Security Officer](../roles/SECURITY_OFFICER.md) | Security, access, data, and anything the reporter can describe in writing | The primary intake. A report sent here is treated as confidential and is not actioned in the open |
| The [Code of Conduct Reporting](../COC_REPORTING.md) process | Conduct concerns, including harassment | Has its own trained handlers and its own confidentiality rules |
| A private message to any maintainer | Where the reporter trusts one specific person | The recipient forwards it to a handler. A maintainer who receives such a report must not sit on it |
| A named external contact, where the project has one | Where the concern is about the project's leadership, or where no internal channel feels safe | External channels are recorded in the project documentation when they exist |
| Anonymous report | Where the reporter will not identify themselves and will not be persuaded to | Accepted. It limits what can be investigated, and that limit is stated plainly rather than used to dismiss the report |

Rules that apply to every channel:

- **A report is never dismissed because it came in the wrong channel.** It is forwarded to
  a handler and the reporter is told where it went.
- **A report is never dismissed because the reporter wants anonymity.** Where anonymity
  makes an investigation impossible, the reporter is told what specifically could not be
  done and why.
- **Reports are not made in public channels.** A report posted in an issue, a pull
  request, or a discussion is moved to a private channel immediately, and the public
  post is removed where the reporter asks. A reporter who posts publicly is treated as
  having made a public disclosure and is told what follows from that, without being
  blamed for it.
- **A reporter may report the same thing twice.** A second report is not treated as
  harassment of the project; it usually means the first was not handled.

## Non-Retaliation Guarantee

This is the core of the policy.

- **No retaliation of any kind** against a person who makes a report in good faith, for
  raising a concern, for participating in an investigation, or for refusing to participate
  in something they believe is wrong.
- **Retaliation is a violation of this policy and of the Code of Conduct**, handled under
  the [Code of Conduct Enforcement](../COC_ENFORCEMENT.md) and the
  [Privilege Revocation Policy](../policies/REVOCATION.md), and it is treated as the most
  serious category of report.
- **Retaliation includes** removing someone's role, reducing their access, reassigning
  their work, rejecting their contributions for improper reasons, excluding them from
  communication, publishing a response that identifies them, and any pressure applied
  through a third party.
- **The guarantee covers good-faith reports that turn out to be unfounded.** A person who
  reports something sincerely and it is not substantiated has not misconducted
  themselves, and the report is closed without any consequence to them. This matters: a
  policy that protects only reporters who were right protects silence.
- **The guarantee extends to witnesses and to people who ask questions.** Asking why a
  decision was made is not retaliation.
- **The project will not require a reporter to prove harm to themselves first.** Retaliation
  is assessed on intent and effect, and a pattern of small acts is assessed as a pattern.
- **Where the project cannot prevent retaliation by someone outside its control** — for
  example an employer or a school — the project will record the concern, act on what it
  can, and say plainly what it cannot do rather than implying protection it does not
  offer.

The [Security Officer](../roles/SECURITY_OFFICER.md) is accountable for this guarantee
and reports on it in the periodic governance review.

## Handling a Report

| Step | What happens | Timing |
| --- | --- | --- |
| 1. Acknowledge | The handler confirms receipt. The reporter is told who now holds the report | Within **2 business days** |
| 2. Triage | Severity assessed and the route decided: conduct, security incident, governance, or financial. Conflicts of interest in the subject matter are identified here | Within **5 business days** |
| 3. Scope | The handler defines what will and will not be investigated, and tells the reporter that scope | Within **10 business days** |
| 4. Investigate | Facts gathered by someone with no conflict of interest. The reporter is told what evidence is being sought | Ongoing |
| 5. Decide | A finding, or a no-finding, with the reasons. Referred to the [Decision Rights Matrix](../matrices/DECISION_RIGHTS_MATRIX.md) where the subject is a maintainer's conduct | Within **30 business days** of scope |
| 6. Act | Action taken under the relevant policy. Where the report concerns the handling team itself, a handler is replaced | With the decision |
| 7. Inform | The reporter is told the outcome. Where confidentiality was promised, the limits of what can be shared are explained | With the decision |
| 8. Record | The report, the decision, and the action are recorded in the [Incident Log](../registers/INCIDENT_LOG.md) without identifying the reporter | Within **10 business days** of the decision |

Rules that apply to the handling:

- **Handlers are conflict-checked.** Anyone named in the report, or with a relationship to
  the subject, does not handle it. Where the concern is about a senior maintainer, the
  handler is someone outside that maintainer's reporting line, or external.
- **Interim measures are available.** Access, roles, or a person's ability to act on the
  project can be suspended while a report is investigated, without that being a finding.
  This is a precaution, and it is reviewed rather than left in place indefinitely.
- **Investigations are proportionate.** The handling aims to establish what happened, not
  to build a case, and it does not extend to unrelated conduct.
- **A report is not shared beyond the handlers.** Confidentiality is maintained for as
  long as possible, and the limits are stated to the reporter rather than assumed.
- **A report is closed, not abandoned.** Where it cannot be progressed, the reason is
  recorded and the reporter is told.
- **Records survive the outcome.** An unsubstantiated report is retained, without
  identifying the reporter, because a pattern of reports is itself information.

## Protection of the Reporter

- **Identity is protected** unless the reporter chooses otherwise, or unless disclosure is
  legally required, in which case the reporter is told before it happens wherever that is
  possible.
- **The reporter is not named** in any record, report, or governance summary.
- **The reporter can request updates** at agreed intervals, and is told if the expected
  timing changes.
- **A reporter facing a risk to their safety or their employment or school position is
  told what the project can do**, which may be limited. Stating a limit honestly is more
  use than implying more protection than exists.
- **A reporter who wants support** is directed to the
  [Code of Conduct Reporting](../COC_REPORTING.md) process, which has trained handlers
  and support contacts.

## Good-Faith Reports

A report is in good faith when the reporter genuinely believes something is wrong,
regardless of whether it proves to be true or whether they were right about the facts.

Reports that are knowingly false, and reports made to harass or to extract something, are
handled differently: the [Code of Conduct Enforcement](../COC_ENFORCEMENT.md) applies. A
person whose genuine concern turns out to be mistaken is not in this category, and the
distinction is drawn deliberately so that the cost of being wrong does not fall on the
person who spoke up.

## Confidentiality and Disclosure

| Situation | Position |
| --- | --- |
| Nothing found | The reporter is told. No finding is published |
| Something found | The action is taken. The reporter is told the outcome; details are shared only as far as confidentiality allows |
| Legal obligation to disclose | Disclosure is made, and the reporter is told beforehand where possible |
| A report that would identify a reporter being published publicly | The publication is prevented or the reporter's identity is redacted, unless disclosure is required |
| Ongoing risk to others, such as an unremediated vulnerability | Disclosure to those at risk is not blocked by a promise of confidentiality to the reporter. The reporter is told it is happening |

The project does not agree to keep a report secret from everyone regardless of
consequences. It agrees not to identify the reporter unnecessarily.

## Enforcement Status

- This is a human process with no automated component. The existence of the channels, and
  the training of handlers, are what make it real.
- No external ethics hotline is operated, and none is claimed. Where the project has no
  external channel, the escalation path reaches the [Community Manager](../roles/COMMUNITY_MANAGER.md)
  and, for concerns about the leadership, the maintainers as a group.
- Handling records are kept in the [Incident Log](../registers/INCIDENT_LOG.md), which is
  reviewed under the [Audit and Assurance Policy](AUDIT_ASSURANCE.md).

## Related Documentation

- [Code of Conduct Enforcement](../COC_ENFORCEMENT.md) and
  [Code of Conduct Reporting](../COC_REPORTING.md)
- [Security Policy](../SECURITY_POLICY.md): the private channel for security reports
- [Incident Log](../registers/INCIDENT_LOG.md) and
  [Conflict-of-Interest Register](../registers/CONFLICT_OF_INTEREST_REGISTER.md)
- [Privilege Revocation Policy](../policies/REVOCATION.md): action taken against a
  person, including for retaliation
- [Conflict Resolution Process](../processes/CONFLICT_RESOLUTION.md) and
  [Escalation Path](../processes/ESCALATION_PATH.md)
- [Audit and Assurance Policy](AUDIT_ASSURANCE.md): how findings are tracked and closed
- [Security Officer role](../roles/SECURITY_OFFICER.md) and
  [Community Manager role](../roles/COMMUNITY_MANAGER.md)
