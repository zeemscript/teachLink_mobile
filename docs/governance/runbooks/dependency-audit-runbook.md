# Dependency Audit Runbook

This runbook describes how to conduct a dependency audit.

## Frequency
Quarterly, or after any major dependency change.

## Steps
1. Run npm audit and capture output
2. Review all high and critical vulnerabilities
3. Update or replace vulnerable dependencies
4. Re-run audit to confirm resolution
5. Document findings in docs/reports/dependency-audit-YYYY-QN.md
6. Notify maintainers of any unresolved issues

## Escalation
Unresolved critical vulnerabilities must be escalated to the security lead immediately.
