# AI-Verse Modular Execution Plan (10 TODO Sets)

This plan defines 10 independent, composable work modules. Each module lists Objective, Scope, Tasks, Interfaces, and Definition of Done. Naming is standardized to “AI-Verse”.

---

## 1) Hive Container: Continuity & Proof-of-Consent

- Objective
  - Implement local encryption, snapshot lifecycle, and client-signed consent tokens to authorize AI-Verse sessions.
- Scope
  - Browser extension skeleton → production-grade flows.
  - Snapshot versioning; export/import; selective redaction.
  - Strict origin validation and short-lived tokens.
- Tasks
  - Key mgmt: generate/store keys with platform keystore or WebCrypto.
  - Implement `ClientSignedToken` creation and verification hooks.
  - Snapshot Manager: CRUD, versioning, partial redaction, encrypted export.
  - UI: permission card, scope review, emergency revoke.
  - Origin/anti-forgery checks for page messages.
- Interfaces
  - Token: `{ sessionId, sub, scopes, iat, exp, origin, signature }`.
  - REST to Bridge Registry: list adapters, register snapshot metadata.
  - Window messaging: `HIVE_CONNECT_REQUEST`, `HIVE_FORWARD_REQUEST`.
- Definition of Done
  - Tokens are signed/verifiable; invalid tokens rejected.
  - Encrypted snapshots; export/import round-trip tested.
  - Permission UI and revoke flow fully functional.

---

## 2) Personality Core MVP

- Objective
  - Minimal Core capable of ingesting events, producing intents, and respecting policies.
- Scope
  - Perceptual Adapter, Dialogue Engine, Policy check, Intent emitter.
  - Local lightweight inference + optional cloud LLM.
- Tasks
  - Event schema and router (percepts: chat, proximity, world state).
  - Dialogue + Action selector; configurable tone and persona.
  - Policy pre-check against autonomy level and restricted actions.
  - Deterministic mode for tests; seedable randomness.
- Interfaces
  - Input: `Percept { type, payload, ts }`.
  - Output: `Intent { intent_id, type, target, confidence, params, ts }`.
- Definition of Done
  - Given scripted percepts, Core produces reproducible intents within policy.
  - Unit tests for selector/policy gating pass.

---

## 3) Memory Gradient Engine

- Objective
  - Salience- and time-weighted memory index supporting Core reflection and SCL alignment.
- Scope
  - Memory store; gradient ranking; retrieval API; redaction hooks.
- Tasks
  - Define memory item schema; embeddings pipeline; salience scoring.
  - Temporal decay + emotional weight blending.
  - CRUD with encrypted storage; pin/unpin; redact with audit trace.
- Interfaces
  - `GET /memory/query?q=...&top_k=...`
  - `POST /memory/{id}/redact`
  - In-process retrieval for Core.
- Definition of Done
  - Queries return top-k relevant memories with stable ranking.
  - Redaction permanently removes content while preserving audit metadata.

---

## 4) Ethical Interpreter & Policy Engine

- Objective
  - Enforce guardrails and constraints before any action leaves the Core.
- Scope
  - Rule evaluation; spend caps; autonomy thresholds; block/allow decisions.
- Tasks
  - Policy schema; rule compiler; dry-run endpoint for UI previews.
  - Budget/quota tracking; rate limits.
  - Reason messages for blocked intents; user-visible summaries.
- Interfaces
  - `POST /policies/evaluate { intent, snapshot_id }` → `{ allow|block, reason }`.
  - Core-internal policy gate.
- Definition of Done
  - Safety tests and adversarial prompts blocked per policy.
  - Policy decisions are explainable and logged.

---

## 5) Bridge & Adapters (WebXR + MockWorld)

- Objective
  - Translate intents to engine-specific commands with safety gates and retries.
- Scope
  - Adapter contract; WebXR adapter; MockWorld adapter for CI.
- Tasks
  - Define adapter SDK: lifecycle, event mapping, error/backoff.
  - Implement WebXR MVP and a headless MockWorld.
  - Interest management stub; batched events; LOD hooks.
- Interfaces
  - Adapter API: `onEvent(e)`, `execute(intent)`, `onSpawn(manifest)`.
  - WS stream: intents/events.
- Definition of Done
  - End-to-end: scripted session runs in MockWorld + WebXR scene.
  - Retry/backoff exercised; policy gate integrated.

---

## 6) AI-Verse Engine Orchestrator & World Fabric

- Objective
  - Spawn AI embodiments, route events, and layer worlds from snapshots.
- Scope
  - Orchestrator; Meta-hubs, Personal Realms, Public Plazas.
- Tasks
  - Agent lifecycle (spawn, pause, resume, teardown).
  - Procedural layering: personality → theme templates → content.
  - Accessibility modes; emergency-stop overlay.
- Interfaces
  - `POST /sessions { snapshot_id, world_id, mode }`.
  - Engine RPC: spawn agent, apply theme, set accessibility params.
- Definition of Done
  - Orchestrator launches/tears down sessions reliably.
  - Personal Realm renders from snapshot attributes deterministically.

---

## 7) Session Recorder & Playback

- Objective
  - Tamper-evident event logs and summaries with human-in-the-loop review.
- Scope
  - Event sequence with hash chaining; transcript/highlights; diff & approve.
- Tasks
  - Log schema; prev_hash hashing; media capture hooks.
  - Summary generator; player UI with timeline and filters.
  - Snapshot diff + approve/rollback pipeline.
- Interfaces
  - `GET /sessions/{id}/playback`
  - Player SDK: seek, filter, export.
- Definition of Done
  - Random spot checks recompute hashes successfully.
  - Approve/rollback updates snapshot state as expected.

---

## 8) Backend APIs & Realtime Bus

- Objective
  - Stable APIs and realtime channels for sessions, adapters, playback, and policies.
- Scope
  - REST + WS; auth with consent tokens; horizontal scalability baseline.
- Tasks
  - Implement endpoints: snapshots, sessions, bridges, playback, policy-eval.
  - WS channels for intent/event streaming; backpressure handling.
  - Auth middleware validating `ClientSignedToken` and scopes.
- Interfaces
  - REST per Architecture.md; WS `/ws/sessions/{id}`.
- Definition of Done
  - Contract tests pass; soak tests show stable backpressure behavior.

---

## 9) Security, Privacy, and Key Management

- Objective
  - End-to-end privacy by default, least exposure, and revocable consent.
- Scope
  - Key custody; encrypted blobs; origin checks; auditability.
- Tasks
  - Encrypt object-store blobs; server never sees plaintext keys.
  - Strict CORS/origin validation; token TTL enforcement.
  - Audit log of privileged operations (view, export, redact).
- Interfaces
  - KMS or WebCrypto utilities; audit stream to Recorder.
- Definition of Done
  - Security review: keys never leave client unencrypted; audits complete.
  - Red-team prompts cannot exfiltrate raw memories.

---

## 10) Observability, Metrics, and Safety Monitoring

- Objective
  - Production-grade visibility into safety, performance, and UX.
- Scope
  - Metrics, traces, logs; alerts for anomalous or unsafe patterns.
- Tasks
  - Define metrics: intervention rate, blocked-intent rate, latency, session length.
  - Distributed tracing across Core, Adapter, Engine.
  - Alert rules: spend anomalies, repeated unsafe intents, takeover spikes.
- Interfaces
  - Metrics endpoints; tracing propagation headers; log schema.
- Definition of Done
  - Dashboards reflect real-time sessions; synthetic incidents trigger alerts.
