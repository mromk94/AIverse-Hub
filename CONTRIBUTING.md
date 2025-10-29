# Contributing to AI-Verse

Thank you for helping build AI-Verse. This guide standardizes docs style, commit conventions, and PR hygiene.

---

## Docs Style Rules (Frozen)
- Canonical naming: use “AI-Verse” (with hyphen) consistently across titles, text, alt text, and code comments.
- JSON contracts: use canonical JSON blocks (no trailing commas). Prefer lowercase snake/camel keys as defined in Architecture.
- Time format: all timestamps are UTC seconds (number), not ISO strings.
- Evolution: prefer additive changes; never change meaning/type of existing fields. Unknown fields must be ignored by receivers. Use `meta` for forward extensions.
- Branding: keep diagrams and headings aligned with repository naming and Glossary.
- Cross-linking: when adding/altering schemas, add links to relevant sections in `docs/ARCHITECTURE.md` and update `docs/EXECUTION_TODOS.md` status as needed.

## Commit Message Convention (Conventional Commits)
- Format: `<type>(optional scope): <subject>`
- Common types: `docs`, `feat`, `fix`, `refactor`, `test`, `chore`.
- Examples:
  - `docs(architecture): add Adapter Capability Matrix`
  - `feat(core): deterministic decision loop seed`
  - `fix(policy): correct rate limit reason code`

## Pull Request Checklist
- [ ] Naming and branding use “AI-Verse” consistently.
- [ ] JSON contracts follow canonical style; timestamps in UTC seconds.
- [ ] Backwards compatible (additive) changes; reason codes unchanged unless explicitly versioned.
- [ ] Cross-links added/updated:
  - [ ] `docs/ARCHITECTURE.md` section(s) linked from README if appropriate.
  - [ ] `docs/EXECUTION_TODOS.md` statuses/links updated.
  - [ ] Glossary updated if new terms introduced.
- [ ] Security/privacy: respects least exposure, no raw memory export; Auth/Token notes align with section 5.6.
- [ ] Observability: include `corr_id`/`trace_id` notes in API examples when relevant.
- [ ] Links checked (no broken anchors); tables render in GitHub preview.

## Development Notes
- Keep edits minimal and incremental (Larry State). Prefer small PRs with a single concern.
- Avoid adding large binaries; place diagrams in `docs/img/` and export as SVG/PNG.
- For new APIs or contracts, provide a short request/response example and reason about error codes.

## Getting Help
- See `docs/GLOSSARY.md` for terminology.
- Architecture reference: `docs/ARCHITECTURE.md`.
- Execution plan: `docs/EXECUTION_TODOS.md`.
