# Access Review Policy

## Purpose

This policy defines how access to TeachLink Mobile's systems is reviewed, how often,
what each review covers, and how access is revoked when it is no longer justified.
Access is granted to people who need it and then kept because removing it is somebody's
job. A scheduled review is that job.

## Scope

This policy covers access to every resource the project relies on:

| Resource | What access means | Register entry |
| --- | --- | --- |
| The repository on GitHub | Read, triage, write, maintain, admin | [Access Register](../registers/ACCESS_REGISTER.md) |
| Continuous integration | Workflow logs, secrets, environment variables, self-hosted runners | [Access Register](../registers/ACCESS_REGISTER.md) |
| EAS and build credentials | Build profiles, submitted builds, credential store | [Access Register](../registers/ACCESS_REGISTER.md) |
| App stores | App record, submission, TestFlight or internal testing | [Access Register](../registers/ACCESS_REGISTER.md) |
| Chat, discussion, and community accounts | Posting, moderation, administration | [Access Register](../registers/ACCESS_REGISTER.md) |
| Domains and hosting | DNS, certificates, deployment | [Access Register](../registers/ACCESS_REGISTER.md) |
| Third-party services | Dashboards, API keys, configuration | [Subprocessor Register](../registers/SUBPROCESSOR_REGISTER.md) and the Access Register |
| Service and machine identities | Non-human credentials under the [Service Accounts Policy](SERVICE_ACCOUNTS.md) | [Access Register](../registers/ACCESS_REGISTER.md) |

It covers the review itself. Removal of a role is
[Contributor Offboarding](../processes/OFFBOARDING.md), and the consequence of removing
elevated privileges is the
[Privilege Revocation Policy](REVOCATION.md).

## Review Cadence

| Resource class | Review frequency | Reviewer |
| --- | --- | --- |
| Repository admin, store accounts, and EAS credentials | **Every 3 months** | [Security Officer](../roles/SECURITY_OFFICER.md) with the [Release Manager](../roles/RELEASE_MANAGER.md) |
| Workflow secrets, environments, and runners | **Every 3 months** | [Security Officer](../roles/SECURITY_OFFICER.md) |
| Maintainer and reviewer privileges | **Every 6 months** | Maintainers, per the [Reviewer](../roles/REVIEWER.md) and [Maintainer](../roles/MAINTAINER.md) roles |
| Triager, moderator, and other elevated roles | **Every 6 months** | [Community Manager](../roles/COMMUNITY_MANAGER.md) or the [Moderator](../roles/MODERATOR.md) as applicable |
| All other contributor access | **Every 12 months** | Maintainers |
| Service accounts and machine identities | **Every 3 months**, alongside the register | [Security Officer](../roles/SECURITY_OFFICER.md) |
| Dormant or unknown accounts | At the next review, without waiting for cadence | Whoever finds them |

The cadence is a floor, not a schedule to be optimised. A review happens when someone
joins, changes role, or leaves, and when a credential or permission changes.

## Scope of the Review

Each review works from the [Access Register](../registers/ACCESS_REGISTER.md), which is
the authoritative list. A resource not in the register is a finding: either the register
is incomplete or the access is unowned.

The reviewer confirms, for every entry:

| Question | Why it matters |
| --- | --- |
| Is this person still in the project? | Access for someone who has left is the most common finding |
| Is the access still required by their current role? | Roles change; permissions usually do not follow automatically |
| Does the level match the role? | Write where read suffices, or admin where write suffices, is over-provisioning |
| Is the grant still within its intended scope? | Access to one repository or environment grew into several without a decision |
| Is the credential within its rotation age? | A grant that outlived its key is a grant nobody is watching |
| Was the grant made by someone with authority? | An unauthorised grant is removed, not ratified silently |
| Is any access shared between people? | Shared credentials defeat attribution and are never permitted |
| Does any service account still match its declared job? | Machine access drifts as the code around it changes |
| Is there a dormant account, an unknown account, or an account for a former contributor? | Dormant access is access nobody is watching |

The reviewer also confirms the reverse: that nobody **lacks** access they need for the
role they hold. A review that only removes is not a review; a person blocked from doing
their job is as much a defect as one who can do too much.

## Revocation Process

Access that fails review is revoked promptly, not queued for the next cycle.

1. **Record the finding.** The entry in the [Access Register](../registers/ACCESS_REGISTER.md)
   is annotated with what was found and the decision.
2. **Reduce or revoke.** Access is reduced to the level the role needs, or revoked. A
   reduction is preferred where the person still needs some of it.
3. **Apply the change at the source.** The role, team membership, or credential is
   removed on the platform, not merely annotated in the register. An entry marked for
   revocation while the access remains live is not done.
4. **Rotate what was shared.** Any credential the departing or reduced person held is
   rotated, per the [Secret Rotation Policy](SECRET_ROTATION.md), because they may still
   hold a copy.
5. **Record the revoke date.** The register entry moves to `Revoked` with the date.
   Entries are never deleted, so the register shows both current and historical access.
6. **Confirm.** The reviewer confirms the access no longer works, rather than assuming
   the platform honoured the change.
7. **Follow up.** Where the finding indicates a process failure — access granted without
   approval, a register not maintained — a process fix is raised, so the same finding is
   not rediscovered next quarter.

### Off the cycle

Revocation is immediate, outside the review cadence, when:

- a person leaves, resigns, or is offboarded, following
  [Contributor Offboarding](../processes/OFFBOARDING.md);
- a role is removed through the [Privilege Revocation Policy](REVOCATION.md);
- a device, account, or credential is lost or suspected compromised;
- an account is found that nobody recognises; or
- a person has been inactive for the period the
  [Inactivity Policy](INACTIVITY.md) sets.

An account with no activity and no identified owner is removed, not retained "in case
it is needed". That phrase is how unowned access persists for years.

## Evidence

A review is evidenced by the completed [Access Register](../registers/ACCESS_REGISTER.md)
entries and a short record stating:

- the date and the reviewers;
- the resources reviewed and the number of entries checked;
- findings raised, with the decision for each;
- any access changed, with before and after state; and
- anything that could not be verified, and why.

The record is retained with the governance review material. A review that leaves no trace
cannot be distinguished from one that never happened, which is why the outcome is written
down even when the outcome is "no changes".

## Enforcement Status

- Access is enforced by the platform's own role model and, where enabled, by branch
  protection rules described in the
  [PR Approval Requirements](APPROVAL_REQUIREMENTS.md#branch-protection-rules).
- No automation enumerates access across systems and compares it to the register. The
  register is a maintained document; its accuracy depends on the review updating it.
- CI workflow permissions are separately reviewed, per the
  [CI/CD Governance Policy](CICD_GOVERNANCE.md), which this policy complements rather
  than duplicates.

## Exceptions

A deliberate exception to a review finding — access retained beyond what the role needs,
or a credential past its rotation age — is recorded in the
[Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md) with the reason, the
compensating control, an approver holding the authority to grant it, and an expiry no
more than 12 months ahead. An exception to a revocation decision is not granted for
privileged access without the Security Officer's approval.

## Ownership and Review

The [Security Officer](../roles/SECURITY_OFFICER.md) owns this policy, with the
[Community Manager](../roles/COMMUNITY_MANAGER.md) responsible for community-platform
access. It is reviewed at least once every 12 months, and after any off-cycle
revocation reveals a gap. Changes are proposed in a pull request that touches only the
`Governance/` folder.

## Related Documentation

- [Access Register](../registers/ACCESS_REGISTER.md): the authoritative list this policy
  operates on
- [Service Accounts Policy](SERVICE_ACCOUNTS.md): machine identities
- [Secret Rotation Policy](SECRET_ROTATION.md): credential age
- [Contributor Offboarding](../processes/OFFBOARDING.md) and
  [Inactivity Policy](INACTIVITY.md): when access ends
- [Privilege Revocation Policy](REVOCATION.md): removing a role
- [CI/CD Governance Policy](CICD_GOVERNANCE.md): workflow permissions and secrets
- [Decision Rights Matrix](../matrices/DECISION_RIGHTS_MATRIX.md) and
  [Maintainers Register](../registers/MAINTAINERS_REGISTER.md)
- [Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md)
- [Security Officer role](../roles/SECURITY_OFFICER.md)
