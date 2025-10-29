# AI-Verse Glossary (Canonical)

Short, frozen definitions to standardize terminology across AI-Verse and Hive Container.

---

## Core Platforms
- **AI-Verse**
  - Human–AI co-experience platform with immersive worlds and the Shared Consciousness Loop (SCL).
- **Hive Container**
  - Client-side continuity and consent layer. Manages keys, encrypted Personality Snapshots, and client-signed consent tokens.
- **Shared Consciousness Loop (SCL)**
  - Feedback system aligning human reflection and AI evolution. Informs memory, ethics, and improvement loops.

## Identity & Continuity
- **Personality Snapshot**
  - Encrypted state representing a user’s AI persona: profile, policies, memory index, artifact references, and versions.
- **ClientSignedToken**
  - Ephemeral proof-of-consent token bound to `sessionId|sub|scopes|iat|exp|origin`, signed by the client (Hive Container).

## AI Runtime (Core)
- **Personality Core**
  - Runtime that perceives, reflects, retrieves memory, decides, passes the policy gate, and emits intents.
- **Percept**
  - Normalized event from engine/user/system (e.g., chat, proximity, world_state). Input to the Core.
- **Intent**
  - Normalized action proposal from Core to Adapters/Engine (e.g., speak, move, animate, buyItem).
- **Policy Decision**
  - Explainable allow/block result with `reason_code` and `checks[]` for a candidate intent.
- **Reason Codes** (examples)
  - `ok`, `blocked_budget`, `blocked_scope`, `restricted_action`, `rate_limited`, `autonomy_violation`, `privacy_violation`, `ethics_violation`, `cooldown`, `requires_user_approval`.

## Memory
- **Memory Gradient Engine**
  - Retrieval system combining semantic similarity, recency decay, salience, emotion, tags, and pinning.
- **Memory Item**
  - Canonical record with `id, ts, text, source, salience, emotion, tags, embedding_ref, privacy, retention, links, checksum, redacted`.

## Bridge & Adapters
- **Bridge Registry**
  - Backend index of available connectors/adapters/worlds.
- **Adapter SDK**
  - Lifecycle for session-bound integrations: `onSpawn`, `onEvent`, `execute`, `getState?`, `dispose`.
- **Adapter Manifest**
  - Spawn-time descriptor: session/world ids, agent avatar/capabilities, policy level, channels.
- **Event Mapping**
  - Environment-specific translation of intents to engine APIs (Unity, WebXR, etc.).

## Engine & Worlds
- **AI-Verse Engine (World Runtime)**
  - Orchestrates AI embodiments, world fabric, and interactions; enforces accessibility and emergency-stop.
- **World Fabric Template**
  - Declarative world description: theme, layered params, spawn points, accessibility, hooks, performance caps.

## Sessions, Recording, Playback
- **Session**
  - Lifecycle of an AI-embodied experience: `creating|running|paused|terminating|terminated|error`.
- **Session Event**
  - Canonical log entry with chain hashing: `hash = SHA-256(canonical_json(event_without_hash))` and `prev_hash`.
- **Recorder**
  - Tamper-evident logging of intents, actions, percepts, policy blocks, and metrics.
- **Playback**
  - API/UI to page through events, media references, and highlights.

## Transport & Realtime
- **Session WS Stream**
  - Framed messages `{type, seq, ts, payload}` for `intent|event|state|error|ack|heartbeat|backpressure` with ordering and backpressure policies.

## Observability & Security
- **Tracing & Correlation**
  - `trace_id`, `span_id`, `parent_span_id` and `corr_id` propagate across HTTP/WS/Recorder/Adapter.
- **Security & Privacy Invariants**
  - Keys stay client-side, least exposure of data, strict origin and scope validation, short TTL tokens, irreversible redaction, transparent audit.

---

References: see docs/ARCHITECTURE.md for canonical schemas and endpoints.
