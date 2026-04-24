---
name: mikos
description: Use when the user asks about Mikos / MIKOSLAB V2 Prompting System, the 8-section UGC structure (Header → Camera → Environment → Subject → Script → Audio → Anti-Glitch → Final Output Feel), or the 16 MIKOSLAB subject archetypes. Stub skill — source material is limited in the current corpus; use the referenced structure as a scaffolding and route active Seedance rendering to `seedanceX` which won head-to-head against `/mikos`.
version: 0.1.0-stub
tested_on: not validated
---

# Mikos / MIKOSLAB V2 (STUB)

Stub skill. Source corpus references the MIKOSLAB V2 system but the full 16-archetype library and the Anti-Glitch section details were not captured in the ingested notes. This skill provides the structural scaffolding; flesh out the archetypes and anti-glitch rules as Yasir sources the original MIKOSLAB V2 document.

## Status

- `/seedanceX` beat `/mikos` head-to-head on Seedance 2.0 (2026-04-14, 8s + 15s renders)
- MIKOSLAB V2's 8-section structure is PRESERVED whole inside the `/ugc` skill (Overseer B's scope)
- This stub exists for the case where the user explicitly asks for Mikos-style rendering

## When to Use

Activate when the user explicitly says:
- "Mikos", "MIKOSLAB V2", "Mikos prompting"
- "8-section UGC prompt", "MIKOSLAB archetype"

In most cases, prefer `seedanceX` (Seedance rendering) or `kling-video` (talking-head) over Mikos. Only use Mikos if the user has a specific reason (legacy workflow, head-to-head benchmarking, archetype-driven creative).

## The 8-Section Structure (used whole — never condense)

```
1. Header
2. Camera
3. Environment
4. Subject
5. Script
6. Audio
7. Anti-Glitch
8. Final Output Feel
```

**Hard rule (locked 2026-04-13):** Mikos 8-section structure is used WHOLE. Never condense or merge sections. This is the one behavior `/ugc` preserves from Mikos.

## 16 Subject Archetypes — TODO

The MIKOSLAB V2 system defines 16 recurring subject archetypes (personas optimized for UGC content on Seedance). The current corpus does NOT enumerate these.

**Action for user:** Provide the original MIKOSLAB V2 Prompting System document so the 16 archetypes can be captured verbatim in `Skills/mikos/archetypes.md`.

## Anti-Glitch Section — TODO

The 7th section of the Mikos structure is "Anti-Glitch" — a set of rules preventing common Seedance/Kling failures during rendering. Content not captured in the current corpus.

**Action for user:** Provide the Anti-Glitch section content for capture in `Skills/mikos/anti-glitch.md`.

## Reference Files

Living in `Skills/mikos/`:
- `notes.md` — stub with structural scaffolding
- `archetypes.md` — empty template for 16 archetypes (to be populated)
- `anti-glitch.md` — empty template for anti-glitch rules (to be populated)

## See also

- `seedanceX` — canonical Seedance prompting (beat `/mikos` head-to-head)
- `kling-video` — canonical Kling prompting
- `ugc` (Overseer B) — preserves the Mikos 8-section structure whole for UGC strategy

## When to Skip

If the user doesn't specifically ask for Mikos rendering, skip this skill and route to `seedanceX` or `kling-video` directly.
