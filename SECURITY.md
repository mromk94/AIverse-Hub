# Security Policy

We take safety, privacy, and consent seriously for AI-Verse. This document explains how to report vulnerabilities and the core data-handling principles that all contributions must respect.

---

## Reporting a Vulnerability
- Prefer using GitHub Security Advisories for private disclosure to maintainers of this repository.
- If email is required, use a private channel and include steps to reproduce and impact. Avoid sharing real user data.
- We will acknowledge receipt and work with you on remediation and coordinated disclosure.

## Scope
- This repository contains documentation and example schemas for AI-Verse.
- Security findings of interest include (non-exhaustive):
  - Origin validation bypasses, consent-token forgery or reuse beyond TTL.
  - Policy Engine bypasses (unsafe intents leaving the Core).
  - Recorder hash-chain tampering or export leakage of non-exportable data.
  - WS stream authentication or backpressure abuse leading to unsafe behavior.

## Data Handling Principles (Frozen)
Align with Architecture sections 7.1–7.4.
- Least exposure: raw memories remain client-side; only intents/actions and derived signals may cross boundaries.
- Consent tokens: short-lived (TTL), bound to origin and subject; rotation and revocation must be supported.
- Key custody: private keys remain client-side; use platform keystores or client-side encryption.
- Redaction & export: redaction is irreversible; exports exclude non-exportable memories; include checksums and manifests.
- Emergency stop: user-triggered STOP suppresses intents, halts actions, revokes session, and logs state change.

## Tokens, Secrets, and PII
- Never commit secrets or provider tokens to the repo.
- Do not handle real PII or raw user memory artifacts in examples or tests.
- Use placeholders and sanitized data for demonstrations.

## Responsible Disclosure
- Provide a clear description, impact, and minimal PoC (sanitized).
- Allow a reasonable window for remediation before any public disclosure.
- Avoid testing against real users or production data.

## References
- Architecture: docs/ARCHITECTURE.md (Security & Privacy Invariants, Rotation & TTL, Emergency-Stop, Retention/Export)
- Glossary: docs/GLOSSARY.md
