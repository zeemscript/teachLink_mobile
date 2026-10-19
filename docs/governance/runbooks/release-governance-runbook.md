# Release Governance Runbook

This runbook covers governance steps required during a release.

## Pre-Release
- Confirm all planned issues are resolved or deferred
- Review open security advisories
- Obtain release approval from at least 1 maintainer

## Release Day
1. Tag the release commit following SemVer
2. Update CHANGELOG.md
3. Publish release notes on GitHub Releases
4. Announce in community channels

## Post-Release
- Monitor for regressions for 48 hours
- Archive the release branch if applicable
