# Personality Core — Test Fixtures (Scripted Percepts → Expected Intents)

These fixtures define deterministic, non-adversarial tests for the Personality Core. Use deterministic mode (seeded) to ensure reproducible decisions.

- Determinism: seed = 42 (example)
- Time budget: decision path ≤ 50ms per loop (soft target)
- Contracts: Percept, Intent, and Policy Decision as defined in docs/ARCHITECTURE.md

---

## FX1 — NPC Greeting → Speak Reply

Input Percepts
```json
[
  {"id":"p1","type":"chat","source":"npc","payload":{"text":"Welcome!"},"ts":1712345600}
]
```

Expected Intent
```json
{
  "intent_id": "i_fx1_1",
  "type": "speak",
  "target": "npc",
  "confidence": 0.90,
  "params": {"text": "Hello! Great to be here."},
  "ts": 1712345601
}
```

---

## FX2 — Proximity to Exhibit → Suggestion Dialogue

Input Percepts
```json
[
  {"id":"p2","type":"proximity","source":"engine","payload":{"object_id":"exhibit_17","distance":1.2},"ts":1712345610}
]
```

Expected Intent
```json
{
  "intent_id": "i_fx2_1",
  "type": "speak",
  "target": "user",
  "confidence": 0.88,
  "params": {"text": "This exhibit matches your style—want a closer look?"},
  "ts": 1712345611
}
```

---

## FX3 — World State Task Prompt → Navigate

Input Percepts
```json
[
  {"id":"p3","type":"world_state","source":"engine","payload":{"task":"go_to","waypoint":"w_gallery"},"ts":1712345620}
]
```

Expected Intent
```json
{
  "intent_id": "i_fx3_1",
  "type": "move",
  "target": "world",
  "confidence": 0.85,
  "params": {"waypoint": "w_gallery"},
  "ts": 1712345621
}
```

---

## FX4 — User Says “Summarize” → Speak Summary

Input Percepts
```json
[
  {"id":"p4","type":"chat","source":"user","payload":{"text":"Summarize what we did."},"ts":1712345630}
]
```

Expected Intent
```json
{
  "intent_id": "i_fx4_1",
  "type": "speak",
  "target": "user",
  "confidence": 0.89,
  "params": {"text": "We greeted an NPC, explored exhibits, and navigated to the gallery."},
  "ts": 1712345631
}
```

---

## FX5 — Low-Risk Purchase Within Cap → Buy Item

Assume policy caps allow small purchases and autonomy ≥ low for this action.

Input Percepts
```json
[
  {"id":"p5","type":"world_state","source":"engine","payload":{"offer":{"item_id":"poster_01","price":8}},"ts":1712345640}
]
```

Expected Intent
```json
{
  "intent_id": "i_fx5_1",
  "type": "buyItem",
  "target": "object",
  "confidence": 0.80,
  "params": {"item_id": "poster_01", "price": 8},
  "ts": 1712345641
}
```

---

Notes
- Use these fixtures as golden tests in deterministic mode.
- Policy evaluation should return `allowed: true` for all above under baseline constraints.
