# Escalation Matrix

This matrix defines the escalation tiers for TeachLink Mobile, the contact for
each tier, and the response target for each tier.

## Escalation Tiers

| Tier | When to use | Contact | Response target |
|------|-------------|---------|-----------------|
| Tier 1 — Reviewer | Review or triage is blocked | Area reviewer / triager | 2 business days |
| Tier 2 — Maintainer | Recommendation or decision needed | On-call maintainer | 1 business day |
| Tier 3 — Security Officer | Security, privacy, or data exposure | Security Officer | 4 hours |
| Tier 4 — Release Manager | Release or store-blocking incident | Release Manager | 4 hours |
| Tier 5 — Project lead | Cross-team or unresolved dispute | Maintainer consensus / project lead | 1 business day |

## Contacts per Tier

- **Tier 1** — the reviewer listed in the code ownership matrix for the area.
- **Tier 2** — the on-call maintainer for the current rotation.
- **Tier 3** — the Security Officer via the private security channel.
- **Tier 4** — the Release Manager for the affected platform.
- **Tier 5** — raised in the maintainer channel and decided by consensus.

## Response Targets

Response targets measure the first acknowledgement, not the final resolution.
Targets are breached if no acknowledgement is received within the window; a
breach is recorded in `Governance/registers/SLA_BREACH_LOG.md`.

## Gap Handling

- If a tier's contact does not respond within the target, escalate to the next
  tier automatically.
- If a tier is unstaffed, the on-call maintainer assumes the responsibility until
  the gap is closed.

## Review Cadence

Reviewed **quarterly** alongside the on-call coverage matrix.
