# Compatibility and Support Window Policy

## Purpose

This policy defines which operating-system versions, device tiers, and runtime versions
TeachLink Mobile supports, how long each window lasts, and what evidence is required
before a version is dropped. A support window that is never shortened turns into
support for everything; one that is shortened without evidence becomes an abandonment.
This document fixes both ends of that trade.

## Scope

This policy covers the **versions the app is built to run on** and the **length of time
each is supported**. It covers:

- iOS and Android OS versions;
- device tiers, which are defined in the [Device Support Policy](../domains/DEVICE_SUPPORT.md);
- the Expo SDK and React Native versions the app is built against;
- the Node and package-manager versions used for local development and CI; and
- browser support for the web export, where the web build is treated as a first-class
  surface.

It does not cover which **release lines** receive security fixes, which is the
[Security Policy](../SECURITY_POLICY.md#supported-versions), or the criteria for a
platform version reaching end of life, which is the
[OS Version Support Policy](../domains/OS_VERSION_SUPPORT.md). This policy sets the
window; those two decide what happens inside and after it.

## Supported Versions

The authoritative current values, all verifiable from the repository:

| Surface | Supported version | Source of truth |
| --- | --- | --- |
| iOS | 15.1 and later | Deployment target in `app.config.ts`, per the [OS Version Support Policy](../domains/OS_VERSION_SUPPORT.md) |
| Android | 7.0 (API level 24) and later | `minSdkVersion` in `app.config.ts` |
| Expo SDK | 54 | `expo` in `package.json` (`~54.0.33`) |
| React Native | 0.81 | `react-native` in `package.json` (`0.81.5`) |
| Node.js | The version used by CI, currently 20 | `.github/workflows/ci.yml` and `engines` in `package.json` |
| Web browsers | The current and previous major versions of Chrome, Safari, Firefox, and Edge | Verified in the `maestro/` flows and manual web export checks |

"Supported" means, concretely, all of the following:

- the app installs and launches on it;
- defects affecting it are triaged and fixed under the
  [Support Policy](SUPPORT.md);
- it is included in the release testing matrix in the
  [Device Support Policy](../domains/DEVICE_SUPPORT.md); and
- it is inside the support window below.

A version that launches but is not in the testing matrix is not "mostly supported".
It is unsupported and untested, which is stated rather than glossed.

## Window Length

| Surface | Support window | Rationale |
| --- | --- | --- |
| Current OS version and the version before it | Full support while the OS vendor ships security updates, and for at least **12 months** after adoption | Security support from the vendor is what determines whether the project can patch the underlying platform |
| An OS version adopted less than 12 months ago | Provisional support | The project may not yet have adoption data; the window is confirmed or shortened at the 12-month review |
| Current release line | Security fixes, per the [Security Policy](../SECURITY_POLICY.md) | The line users are told to update to |
| Previous release line | Security fixes only, for as long as the store's propagation requires | Store submission lead time, not preference |
| Expo SDK and React Native version | Adopted within the upstream support window, and no longer than the upstream's own end-of-support date | The project cannot patch a runtime that upstream has ended |
| Node.js version for contributors | Current and active LTS | Contributors should not be pinned to an unmaintained runtime |
| Web browsers | Current and previous major version | Balances coverage against the cost of testing more engines |

Rules that qualify the windows:

- **The upstream floor is absolute.** The project does not ship a build targeting an OS
  below the Expo SDK floor, regardless of how many users remain there. A supported
  version the toolchain cannot build is not supported.
- **A window is measured from adoption, not from announcement.** Adoption is the release
  in which the version is first shipped, not the issue in which it was proposed.
- **Windows do not run in parallel indefinitely.** At most two OS major versions are
  supported per platform at a time.
- **An adopted version is not withdrawn early.** Removing support inside the 12-month
  window requires the drop criteria below to be met and a documented exception.

## Drop Criteria

A version is dropped only when at least one criterion is met and the evidence is
recorded. Meeting a criterion starts the notice period; it does not drop the version on
the spot.

| Criterion | Evidence required |
| --- | --- |
| The upstream runtime no longer supports it | The Expo SDK or React Native release in use states that the version is below its floor |
| The OS vendor has ended security updates | Vendor announcement, plus the version falling below 1 percent of sessions in the preceding 90 days |
| A required API is unavailable or blocked and no workaround exists | Documented attempt at the workaround, and confirmation that the platform team agrees no workaround exists |
| A critical vulnerability cannot be mitigated on the version | Security Officer determination that continued support creates unacceptable risk |
| A device tier fails the testing matrix irrecoverably | The [Device Support Policy](../domains/DEVICE_SUPPORT.md) matrix showing the failure, and a confirmed root cause in the platform rather than in the app |
| The store itself has withdrawn support | Store deprecation notice or minimum-version enforcement |

Rules for dropping:

- **Evidence is required, not asserted.** A claim of low usage is paired with the
  session data and the period measured. Where the project has no usage data, the issue
  states that and the criterion is treated as unmet.
- **Adoption data is measured over a stated period.** A percentage is accompanied by the
  window it covers, so a momentary dip cannot drop a version.
- **The notice period still applies.** A drop meets the criteria, then waits the 90 days
  the [Deprecation Policy](DEPRECATION.md#notice-period) requires, before removal takes
  effect. A security-driven drop may shorten the period with a recorded exception, and
  the [OS Version Support Policy](../domains/OS_VERSION_SUPPORT.md) describes that path.
- **Removal is a separate change.** Passing a criterion is not permission to delete the
  support code in the same pull request that noticed the criterion.

## Monitoring and Review

| Activity | Cadence | Owner |
| --- | --- | --- |
| Confirm adoption data for the oldest supported version | Every 90 days | Maintainers |
| Confirm the upstream SDK floor has not moved | Every 90 days | Mobile platform maintainers |
| Review the window for each surface | Every 12 months | [Release Manager](../roles/RELEASE_MANAGER.md) |
| Verify the declared versions still match the repository | Every 12 months | Mobile platform maintainers |

The version table above is verified against `app.config.ts` and `package.json` at each
review, and corrected in the same pull request if it has drifted. A compatibility policy
that states a version the build does not target is worse than one that states nothing.

## Change Control

Raising a minimum version, lowering one, or changing a window is a change with
consequences for users, so:

- it is proposed in a pull request touching only the `Governance/` folder, for the
  policy change itself;
- the removal of support for a version is a separate, code-affecting change following
  the [Deprecation Policy](DEPRECATION.md) and the
  [OS Version Support Policy](../domains/OS_VERSION_SUPPORT.md);
- both are referenced from the same tracking issue, so the policy and the code move
  together rather than diverging.

## Exceptions

A window length that cannot be met for a documented reason — a vendor delay, an upstream
release that arrives without the promised support, a store policy that forces a
shorter window — is recorded in the
[Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md) with the reason,
the compensating control, an approver holding the authority to grant it, and an expiry
no more than 12 months ahead.

## Enforcement Status

- Support levels are enforced by the release testing matrix in the
  [Device Support Policy](../domains/DEVICE_SUPPORT.md) and by the Maestro flows in
  `maestro/`, which are real and run in CI. Whether the current matrix covers every
  version named above is verified by maintainers rather than by an automated check.
- CI runs on a fixed Node version from the workflow definition, so the developer-runtime
  requirement is enforced for CI but not for local machines.
- No automation monitors OS adoption share or detects an upstream SDK floor change. The
  90-day monitoring activities above are human.

## Ownership and Review

The mobile platform maintainers own this policy with the
[Release Manager](../roles/RELEASE_MANAGER.md). It is reviewed when the project adopts
an Expo SDK or React Native release, at each 12-month review, and whenever a drop
criterion is met. Changes are proposed in a pull request that touches only the
`Governance/` folder.

## Related Documentation

- [OS Version Support Policy](../domains/OS_VERSION_SUPPORT.md): minimum versions, drop
  criteria, and notice period
- [Device Support Policy](../domains/DEVICE_SUPPORT.md): device tiers and the testing
  matrix
- [Security Policy](../SECURITY_POLICY.md): which release lines receive fixes
- [Deprecation Policy](DEPRECATION.md) and
  [Deprecation Register](../registers/DEPRECATION_REGISTER.md)
- [End-of-Life Policy](END_OF_LIFE.md) and
  [Support Policy](SUPPORT.md)
- [Versioning Policy](VERSIONING.md) and
  [Release Manager role](../roles/RELEASE_MANAGER.md)
- [Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md)
