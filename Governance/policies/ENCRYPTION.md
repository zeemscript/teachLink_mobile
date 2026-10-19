# Encryption Policy

## Purpose

This policy states what TeachLink Mobile must encrypt, in transit and at rest, which
algorithms are approved, and how an exception is requested. The app handles data about
teachers and children, so an unencrypted copy of a token, a student record, or a
message is a breach even when nothing else in the system is compromised.

## Scope

This policy covers the TeachLink Mobile applications built from this repository, the
API and WebSocket endpoints they talk to, and the local storage on a user's device. It
applies to every new feature, dependency, and configuration that stores or transmits
data.

It governs the choice and use of algorithms. The custody of the keys themselves is
governed by the [Key Management Policy](KEY_MANAGEMENT.md), certificates by the
[Certificate Management Policy](CERTIFICATE_MANAGEMENT.md), and app signing material by
the [App Signing Key Policy](../domains/APP_SIGNING_KEYS.md).

## In-Transit Requirements

| Requirement | Detail |
| --- | --- |
| Transport protocol | TLS 1.2 or later for every HTTPS or WebSocket connection. Plain `http://` and `ws://` are not permitted against any non-local host |
| Certificate validation | Full chain validation is required. Trust-all, custom trust anchors, and disabled hostname verification are not permitted in shipped code |
| Certificate pinning | Not required by default. Where pinning is used, at least one backup pin is configured, and the pin set has an expiry and an owner, or it becomes an outage risk when the CA rotates |
| Local development | Plaintext to `localhost`, `127.0.0.1`, or an emulator loopback address is permitted in development builds only, and must never be the value of a production build |
| WebSocket transport | Secure WebSocket, as in the `wss://api.teachlink.com` value in `.env.example` |
| Credentials in transit | Access and refresh tokens travel in headers, never in a URL query string, which is logged by proxies and servers |
| Third-party endpoints | Every third-party endpoint the app contacts must be HTTPS, and each is recorded as a subprocessor in the [Subprocessor Register](../registers/SUBPROCESSOR_REGISTER.md) |

A URL that falls back to `http://` for images, as `src/components/ui/CachedImage.tsx`
allows, is a finding under this policy: a downgrade path that exists for one asset class
exists for all of them. New code may not add such a fallback, and existing cases are
fixed opportunistically as required by the
[Deprecation Policy](DEPRECATION.md).

## At-Rest Requirements

| Data | Where it lives | Requirement |
| --- | --- | --- |
| Access token, refresh token, session data | `src/services/secureStorage.ts` | Platform-protected storage only: iOS Keychain with `WHEN_UNLOCKED_THIS_DEVICE_ONLY`, Android Keystore through `expo-secure-store`. No plaintext fallback |
| Cached feature data | `src/utils/encryptedStorage.ts` | Encrypted with AES-GCM 256 before being written to `AsyncStorage`. The data key is generated with `expo-crypto` and held in protected storage, not alongside the ciphertext |
| Offline lesson and record content | Offline cache | Encrypted at rest; the cache is described in [Offline Data Governance](../domains/OFFLINE_DATA.md) |
| Non-sensitive preferences | `AsyncStorage` | Permitted without encryption, provided the value is genuinely non-sensitive and contains nothing about a person |
| Crash reports and telemetry | Sentry and similar | Must not contain personal data; see [Mobile Telemetry Governance](../domains/MOBILE_TELEMETRY.md) |
| Device backups | Platform backup | Token storage must be excluded from backup by the platform configuration, so that a restored device cannot resurrect a session |
| Build artifacts and logs | CI | Must not contain user data or credential values, as required by the [CI/CD Governance Policy](CICD_GOVERNANCE.md) |

Two rules are absolute and are checked in code review:

- **No sensitive value in plaintext storage.** There is no fallback path to
  `AsyncStorage`, `SharedPreferences`, or `UserDefaults` for a token, a credential, or a
  record. A write that cannot be protected fails loudly rather than degrading.
- **No sensitive value in a log.** Logging a token, a password, a full personal
  record, or an encryption key is a defect regardless of storage. See
  `src/services/secureStorage.ts`, where the storage layer documents the anti-patterns
  it avoids.

Data classified as non-sensitive is only non-sensitive while it stays non-sensitive.
Promoting an existing `AsyncStorage` value to hold user data is a change under this
policy and moves it into the table above.

## Approved Algorithms

| Use | Approved | Not approved |
| --- | --- | --- |
| Symmetric encryption | AES-GCM (256-bit preferred) or AES-GCM 128 where a platform constrains key size, with an authenticated tag | AES in ECB or CBC without authentication, 3DES, DES, RC4 |
| Asymmetric encryption | RSA with 2048 bits or more, or ECDSA P-256 and above | RSA below 2048 bits, DSA |
| Key agreement | ECDH on P-256 or X25519, or the platform TLS 1.3 key exchange | Static DH parameters, custom hand-rolled key exchange |
| Hashing | SHA-256 and SHA-3 | MD5, SHA-1 for any security purpose |
| Password or secret derivation | A memory-hard KDF such as Argon2id, scrypt, or PBKDF2 with a high iteration count, per current guidance | Bare SHA-256 of a password, a single round of any hash |
| Randomness | The platform CSPRNG, through `expo-crypto` or `crypto.getRandomValues` | `Math.random()` for anything security-relevant, including session identifiers and IVs |
| Transport | TLS 1.2 or 1.3 with suites the platform considers secure | Custom TLS, certificate pinning without a backup pin |

Rules that apply to every algorithm:

- **Authenticated encryption only.** A mode without an authentication tag is not
  approved, because an attacker who can modify ciphertext would otherwise be
  undetectable.
- **A fresh nonce or IV per encryption,** never reused with the same key. `encryptedStorage.ts`
  generates a nonce per write; a change that caches a nonce is a defect.
- **No bespoke cryptography.** Algorithms come from the platform or a reviewed library.
  A hand-rolled cipher, padding scheme, or key derivation in this repository is not
  approved regardless of merit, because it cannot be reviewed or upgraded.
- **Algorithm agility.** New code must use named algorithms rather than a negotiated
  default, so that a weak choice is visible in review.
- **Downgrade resistance.** Code must not silently fall back to a weaker algorithm if
  the preferred one is unavailable; it must fail closed.

Dependency changes are also checked against the
[Dependency Register](../registers/DEPENDENCY_REGISTER.md) and the licence allowlists,
because a crypto library's licence and maintenance status affect whether it may be
adopted.

## Exception Process

An exception is needed when a requirement above cannot be met. The process is
deliberate, and a request that cannot answer the questions below is refused rather than
granted conditionally.

1. **Open a request.** An issue states the requirement, the file or endpoint affected,
   and the concrete reason the approved option is unavailable.
2. **State the risk.** The request describes what an attacker gains from the deviation
   and why the risk is acceptable, including the data involved and who it belongs to.
   "It would be faster" and "the library does not support it" are not risk statements.
3. **Propose a mitigation.** Every request names a compensating control: a shorter
   session lifetime, an additional access restriction, monitoring, or a planned
   remediation with a date.
4. **Propose an expiry.** Exceptions expire. A request without an expiry date is
   refused, because it cannot be distinguished from a permanent policy change.
5. **Approve.** The Security Officer approves exceptions affecting credentials, user
   data, or transport. Maintainers approve those affecting only non-sensitive local
   state. Where children's data is involved, the [Children's Data Policy](../compliance/CHILDRENS_DATA.md)
   applies and approval additionally requires that policy's conditions to be met.
6. **Record.** The exception is entered in the
   [Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md) with an approver
   holding the authority to grant it and an expiry no more than 12 months ahead.
7. **Link it in code.** The deviation carries a comment naming the exception ID, so a
   future reader finds the decision rather than assuming an oversight.
8. **Review before expiry.** The approver is reminded before the expiry date. The
   exception is then closed or re-approved with fresh justification, or escalated
   through the [Escalation Path](../processes/ESCALATION_PATH.md).

Renewal is not automatic. A second approval of the same exception is a signal that the
underlying constraint should be fixed, and the approver is expected to say so.

## Enforcement Status

- Encryption choices are enforced by code review and by the unit tests around
  `secureStorage`, `encryptedStorage`, and `initSecureStorage`. There is no automated
  check that scans for a forbidden algorithm or a plaintext write, so a regression
  depends on a reviewer noticing it.
- Transport enforcement depends on the configured `EXPO_PUBLIC_API_BASE_URL` and
  `EXPO_PUBLIC_SOCKET_URL` values being HTTPS in production builds. A misconfigured
  build is a configuration error rather than a code defect, and is checked at build
  time by `npm run validate-env`.
- An automated secret and weak-crypto scan is a reasonable follow-up and would be
  introduced under the [Bot and Automation Governance Policy](BOT_AUTOMATION.md).

## Ownership and Review

The Security Officer owns this policy with the maintainer team. It is reviewed when the
approved-algorithm list changes, when a new storage mechanism is introduced, when a
provider changes its transport requirements, and at least once every 12 months.
Changes are proposed in a pull request that touches only the `Governance/` folder.

## Related Documentation

- [Key Management Policy](KEY_MANAGEMENT.md): custody, access, and revocation of the
  keys used here
- [Certificate Management Policy](CERTIFICATE_MANAGEMENT.md): TLS and signing
  certificates
- [Secret Rotation Policy](SECRET_ROTATION.md): rotation of the credentials that
  transport protects
- [App Signing Key Policy](../domains/APP_SIGNING_KEYS.md): build and store signing
- [Offline Data Governance](../domains/OFFLINE_DATA.md): what the offline cache holds
- [Mobile Telemetry Governance](../domains/MOBILE_TELEMETRY.md): what may leave the
  device
- [Children's Data Policy](../compliance/CHILDRENS_DATA.md): the heightened standard
  for data about children
- [Security Policy](../SECURITY_POLICY.md): reporting a suspected exposure
- [Exception and Waiver Register](../registers/EXCEPTION_REGISTER.md) and
  [Security Exceptions Register](../registers/SECURITY_EXCEPTIONS_REGISTER.md)
