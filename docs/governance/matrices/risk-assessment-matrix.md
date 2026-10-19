# Risk Assessment Matrix

This matrix defines how the project assesses the likelihood and impact of a risk
and how it responds to each resulting score band.

## Axes

**Likelihood** — the probability the risk occurs within the review period.

| Rating | Meaning |
|--------|---------|
| 1 — Unlikely | No realistic path to occurrence. |
| 2 — Possible | Could occur but is not expected. |
| 3 — Likely | Expected to occur at some point. |

**Impact** — the severity of harm if the risk occurs.

| Rating | Meaning |
|--------|---------|
| 1 — Low | Minor inconvenience; recoverable quickly. |
| 2 — Medium | Noticeable disruption to users or contributors. |
| 3 — High | Data loss, security exposure, store removal, or release outage. |

## Scoring Bands

The score is `likelihood x impact`, giving a range of 1 to 9.

| Score | Band | Response |
|-------|------|----------|
| 1–2 | Low | Accept and monitor; review at the next cycle. |
| 3–4 | Medium | Mitigate with a tracked issue and an owner; review quarterly. |
| 6 | High | Mitigate before the next release; maintainer sign-off required. |
| 9 | Critical | Stop-ship; escalate to the security officer immediately. |

## Response per Band

- **Low** — no action required beyond recording the risk in the risk register.
- **Medium** — assign an owner and a target date; re-score at the next review.
- **High** — implement a mitigation and require two maintainer approvals.
- **Critical** — follow the escalation path and the vulnerability disclosure
  process if security is involved.

## Review Cadence

Reviewed **quarterly** by the maintainers, and after any incident that changes
the risk profile. New risks are scored using this matrix as they are identified.
