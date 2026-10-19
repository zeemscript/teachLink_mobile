# Risk Register

This register records the known risks that could affect TeachLink Mobile. Its
purpose is to make risks visible, scored, owned, and reviewed on a predictable
cadence so the project can act before a risk becomes an incident.

## Register Columns

Each risk records the following columns:

| Column | Description |
|--------|-------------|
| Risk ID | Sequential identifier (for example, `RISK-0001`). |
| Description | Short statement of the risk and what it could affect. |
| Category | Security, privacy, availability, delivery, compliance, or people. |
| Likelihood | Probability rating (see scoring). |
| Impact | Consequence rating (see scoring). |
| Score | Likelihood × Impact. |
| Rating | Derived severity band (see scoring). |
| Mitigation | Action that reduces likelihood or impact. |
| Owner | Role accountable for the mitigation. |
| Status | Open, Mitigating, Accepted, or Closed. |
| Last reviewed | Date of the most recent review. |

## Risk Scoring

Likelihood and impact are each rated on a 1–5 scale and multiplied for a score of
1 to 25.

| Rating | Score | Response |
|--------|-------|----------|
| Critical | 15–25 | Mitigate before release; escalate to maintainers. |
| High | 8–14 | Mitigate with a dated plan. |
| Medium | 4–7 | Monitor and mitigate when capacity allows. |
| Low | 1–3 | Accept and record. |

## Register

| Risk ID | Description | Category | L | I | Score | Rating | Owner | Status |
|---------|-------------|----------|---|---|-------|--------|-------|--------|
| _none recorded_ | | | | | | | | |

## Review and Ownership

- Risks are reviewed **monthly** by the maintainers; Critical and High risks are
  reviewed at every release.
- The Security Officer co-owns security and privacy risks.
- Every risk has a named owner; an unowned risk is escalated to the maintainers.

## Related Documents

- `Governance/policies/APPROVAL_REQUIREMENTS.md`
- `docs/governance/matrices/risk-assessment-matrix.md`
