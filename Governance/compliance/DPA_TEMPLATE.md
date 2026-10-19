# Data Processing Agreement Template

## Purpose

A school, district, or other organisation that uses TeachLink Mobile with personal data
usually needs a written data processing agreement (DPA). This document is the project's
**template** for that agreement. It lists the clauses a DPA must contain, how the
subprocessor section works, and the signature fields, so that every agreement starts
from the same baseline and a customer does not have to invent one.

> This is a template to be adapted, **not legal advice**. Square-bracketed text is a
> placeholder. Before a DPA is signed, the maintainers should have it reviewed by
> qualified counsel, and where the law of the customer's jurisdiction requires
> different or additional terms, those prevail.

## Scope and Use

TeachLink Mobile is a project with self-hosted and customer-operated deployments, so
who is the *controller* and who is the *processor* depends on the deployment. The
template assumes the common case:

| Role | Party |
| --- | --- |
| Controller | The customer (the school, district, or organisation) |
| Processor | The party that operates the service for the customer, named in the agreement |
| Data subjects | Students, teachers, administrators, and parents or guardians |

If the customer self-hosts and no one else processes the data, no DPA with the project
is needed; this template is for cases where a named operator processes data for the
customer. Data about children is also subject to the
[Children's Data Policy](CHILDRENS_DATA.md), whose rules the agreement must not weaken.

## Required Clauses

Every DPA made from this template contains, at minimum:

| # | Clause | What it states |
| --- | --- | --- |
| 1 | **Parties and roles** | Who the controller and processor are, with contact details |
| 2 | **Subject matter, duration, nature, and purpose** | What processing is done, for how long, and only for the customer's educational purpose |
| 3 | **Types of data and data subjects** | The categories of personal data and of people, taken from the [privacy manifest](../domains/PRIVACY_MANIFEST.md), including whether child data is involved |
| 4 | **Documented instructions** | The processor acts only on the controller's written instructions and tells the controller if an instruction appears unlawful |
| 5 | **Confidentiality** | People who process the data are bound by confidentiality |
| 6 | **Security measures** | The technical and organisational measures that apply, referring to the [Security Policy](../SECURITY_POLICY.md) and to encryption, access control, and logging in the deployment |
| 7 | **Subprocessors** | See [Subprocessor Section](#subprocessor-section) |
| 8 | **International transfers** | Where data may be processed, and the transfer mechanism (for example standard contractual clauses) where data leaves the customer's jurisdiction |
| 9 | **Assistance** | The processor helps the controller respond to requests from data subjects (access, correction, deletion, portability) and to its own legal duties |
| 10 | **Personal data breach** | The processor notifies the controller **without undue delay and within [72] hours** of becoming aware, with the facts the controller needs to meet its own duties |
| 11 | **Audit** | The controller may audit compliance, or rely on an independent report, on reasonable notice |
| 12 | **Return and deletion** | At the end of the service, data is returned or deleted, as the controller chooses, within [30] days, and deletion is confirmed in writing |
| 13 | **Children's data** | Consent and authorisation obligations and minimisation rules for child accounts are honoured as in the Children's Data Policy |
| 14 | **Liability and indemnity** | As agreed by the parties |
| 15 | **Term and termination** | The agreement lasts as long as the processing, and the duties on confidentiality, deletion, and breach survive it |
| 16 | **Governing law and disputes** | The law and forum chosen by the parties |

A clause may be made stricter for a customer. It may not be removed or weakened below
what the applicable law requires.

## Subprocessor Section

The processor may use subprocessors only as follows:

- **Authorisation.** The controller gives general written authorisation for the
  subprocessors listed in Annex B, which is part of the agreement.
- **Flow-down.** Each subprocessor is bound by data protection terms no less protective
  than the DPA, and the processor remains responsible for its subprocessors.
- **Change notice.** The processor gives the controller at least **[30] days'** written
  notice before adding or replacing a subprocessor. The controller may object on
  reasonable data protection grounds; if the parties cannot agree, the controller may
  terminate the affected service.
- **Source of the list.** Annex B is derived from the declared third-party SDKs and
  services in the privacy manifest, not written from memory. A subprocessor missing
  from the manifest is a defect to be fixed in both places.

Annex B is laid out as:

| Subprocessor | Service provided | Personal data involved | Location | Transfer mechanism |
| --- | --- | --- | --- | --- |
| [Name] | [e.g. crash reporting] | [categories] | [region] | [mechanism] |

The observability tools named in the [Principles](../PRINCIPLES.md) are candidates for
this list in a deployment that uses them; whether they are used, and in what form, is
a fact to confirm against the manifest for that deployment.

## Annexes

| Annex | Content |
| --- | --- |
| **A** | Description of processing: purposes, categories of data and data subjects, duration |
| **B** | Approved subprocessors (table above) |
| **C** | Security measures in force for the deployment |
| **D** | Instructions from the controller, if any beyond the agreement |

## Signature Fields

```text
Controller:  [Organisation legal name]
Signed by:   ______________________   Name: [Authorised signatory]
Title:       [Title]                    Date: [YYYY-MM-DD]
Contact:     [Email for data protection matters]

Processor:   [Operator legal name]
Signed by:   ______________________   Name: [Authorised signatory]
Title:       [Title]                    Date: [YYYY-MM-DD]
Contact:     [Email for data protection matters]
```

Both signatories confirm they are authorised to bind their organisation. The signed
copy is kept by each party; the project does not store customers' signed agreements in
the repository, which is public.

## Maintaining the Template

| Question | Answer |
| --- | --- |
| Who owns it | The [Maintainers](../roles/MAINTAINER.md), with the [Security Officer](../roles/SECURITY_OFFICER.md) reviewing the security and breach clauses |
| When it changes | When the privacy manifest, the Children's Data Policy, or applicable law changes in a way that affects a clause; and at least once a year |
| How it changes | A pull request touching only `Governance/`, with the usual review |
| Versioning | The template carries a version and date. An agreement states which version it was made from |

## Related Documentation

- [Children's Data Policy](CHILDRENS_DATA.md)
- [Privacy Manifest Policy](../domains/PRIVACY_MANIFEST.md)
- [Security Policy](../SECURITY_POLICY.md)
- [Principles](../PRINCIPLES.md)
