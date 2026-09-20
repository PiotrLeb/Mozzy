# Security Policy

We take the security of this project and its users seriously. This document explains how to report vulnerabilities and what security practices we follow.

## Table of Contents

1. [Supported Versions](#1-supported-versions)
2. [Reporting a Vulnerability](#2-reporting-a-vulnerability)
3. [Response Process and Timelines](#3-response-process-and-timelines)
4. [Scope](#4-scope)
5. [Security Practices for Contributors](#5-security-practices-for-contributors)
6. [Dependency Management](#6-dependency-management)
7. [Secrets Management](#7-secrets-management)
8. [Mobile App Security](#8-mobile-app-security)
9. [Incident Response](#9-incident-response)

---

## 1. Supported Versions

Security fixes are provided for the following versions:

| Version | Supported |
|---------|-----------|
| Latest release | Yes |
| Previous minor release | Yes (critical fixes only) |
| Older versions | No |

Please update to the latest version before reporting an issue.

---

## 2. Reporting a Vulnerability

**Do not report security vulnerabilities through public GitHub issues, discussions, or pull requests.**

Use one of these private channels instead:

1. **GitHub Private Vulnerability Reporting** (preferred): go to the **Security** tab of this repository and click **Report a vulnerability**.
2. **Email:** [security@your-domain.com]

### What to include

- A clear description of the vulnerability and its potential impact
- Steps to reproduce (proof of concept, if possible)
- Affected version(s), platform(s), and environment
- Any suggested fix or mitigation
- Your contact details, if you want to be credited

Please do not include real user data or credentials in your report.

---

## 3. Response Process and Timelines

| Stage | Target timeline |
|-------|-----------------|
| Acknowledgement of your report | within 3 business days |
| Initial assessment and severity rating | within 7 business days |
| Status updates | at least every 14 days |
| Fix for critical/high severity | as soon as possible, target within 30 days |
| Fix for medium/low severity | next scheduled release |

These are targets, not guarantees. Complex issues may take longer, and we will keep you informed.

### Coordinated disclosure

- We ask that you give us reasonable time to fix the issue before any public disclosure.
- Once a fix is released, we will publish a security advisory.
- With your permission, we will credit you in the advisory.

### Safe harbor

We will not pursue legal action against researchers who:

- Act in good faith and follow this policy
- Avoid privacy violations, data destruction, and service disruption
- Only interact with accounts and data they own or have explicit permission to test
- Give us reasonable time to respond before disclosing publicly

---

## 4. Scope

### In scope

- Source code in this repository
- Build and release configuration (CI/CD workflows, scripts)
- Official releases and builds of the application
- Official API endpoints and backend services operated by the project

### Out of scope

- Vulnerabilities in third-party dependencies with no demonstrated impact on this project (report those upstream)
- Social engineering, phishing, or physical attacks
- Denial-of-service and volumetric attacks
- Automated scanner output without a demonstrated, exploitable impact
- Issues requiring a rooted/jailbroken device or physical access to an unlocked device
- Missing security headers or best-practice hardening without a concrete exploit
- Reports about outdated versions that are no longer supported

---

## 5. Security Practices for Contributors

All contributors must follow these rules:

- **Never commit secrets:** API keys, tokens, passwords, certificates, private keys, or `.env` files.
- **Validate all input** coming from users, APIs, deep links, and storage. Treat it as untrusted.
- **Do not log sensitive data:** passwords, tokens, personal data, payment details.
- **Use HTTPS/TLS** for all network communication. No cleartext traffic.
- **Follow the principle of least privilege** for permissions, API scopes, and tokens.
- **Do not disable security checks** (certificate validation, lint security rules, CI scans) to make something work.
- **Avoid unsafe patterns:** `eval`, dynamic code execution, unsanitized HTML rendering, string-built queries.
- **Prefer well-maintained, widely used libraries** over custom cryptography or authentication code. Never implement your own crypto.
- **Security-relevant changes** (auth, storage, networking, permissions, payments) require review by at least one maintainer.

Security is part of the code review checklist. Reviewers should ask: does this change handle untrusted input, sensitive data, or permissions?

---

## 6. Dependency Management

- **Dependabot** (or an equivalent tool) is enabled for security and version updates.
- **Audit dependencies** regularly (`npm audit` / `yarn audit`) and in CI. High and critical findings block merges unless explicitly accepted by a maintainer.
- **Commit lockfiles** (`package-lock.json` / `yarn.lock`) and use them for reproducible installs.
- **Review new dependencies** before adding them: maintenance status, popularity, license, known vulnerabilities, and necessity. Fewer dependencies means a smaller attack surface.
- **Pin versions** of critical packages and update them deliberately.
- **Remove unused dependencies.**

---

## 7. Secrets Management

- Secrets are stored in environment variables or a secrets manager, never in the repository.
- `.env` files are listed in `.gitignore`. Only `.env.example` (with no real values) is committed.
- CI/CD secrets are stored in **GitHub Actions secrets**, scoped to the environments that need them.
- **Secret scanning** and **push protection** are enabled on the repository.
- Different secrets are used for development, staging, and production.
- **If a secret is leaked:**
  1. Revoke and rotate it immediately
  2. Remove it from the codebase
  3. Inform the maintainers
  4. Note that deleting it from git history is not enough, because it must be treated as compromised

---

## 8. Mobile App Security

Specific rules for the React Native application:

- **Secure storage:** store tokens and sensitive data in the platform keychain/keystore (e.g. `react-native-keychain` or `expo-secure-store`). Never in `AsyncStorage` or plain files.
- **No secrets in the app bundle.** Anything shipped in the client can be extracted. Use a backend for operations requiring real secrets.
- **Network security:**
  - HTTPS only (disable cleartext traffic on Android, keep ATS enabled on iOS)
  - Consider certificate pinning for sensitive APIs
- **Authentication:** use short-lived access tokens with refresh tokens. Support secure logout and token revocation.
- **Deep links and intents:** validate and sanitize all parameters. Never trust them to perform sensitive actions without user confirmation.
- **Permissions:** request only the permissions the app needs, at the moment they are needed.
- **Release builds:**
  - Disable debug mode and remote debugging
  - Remove development-only code and logs
  - Enable code shrinking/obfuscation (Hermes bytecode, ProGuard/R8) where appropriate
- **Signing keys** (keystore, provisioning profiles) are stored securely, backed up, and never committed to the repository.
- **Sensitive screens:** consider hiding content in the app switcher and preventing screenshots where it makes sense.
- **Local data:** minimize what is stored on the device and clear it on logout.

---

## 9. Incident Response

If a security incident is confirmed:

1. **Contain:** limit the impact (revoke credentials, disable affected features, roll back if needed).
2. **Assess:** determine scope, affected users, and data involved.
3. **Fix:** develop and test a patch.
4. **Release:** ship the fix and, if needed, force or recommend an update.
5. **Communicate:** publish a security advisory and notify affected users where required (including regulatory notification obligations such as GDPR, if applicable).
6. **Review:** run a post-incident review and update processes to prevent recurrence.

---

## Contact

- Security reports: [security@your-domain.com]
- General questions: open a GitHub Discussion or contact the maintainers

Thank you for helping keep this project and its users safe.
