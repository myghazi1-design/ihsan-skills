---
type: skill-reference
skill: video-dialogue-prompting
source: Dialogue Syntax.md (video production:ai source corpus)
model: Kling 3.0 native audio (fal.ai reference)
tags: [dialogue, lip-sync, kling, fal-ai]
---

# Dialogue Syntax — Core Format + P1–P4 Principles

**Confidence: [High] on the character-label format, [Test first] on the "Immediately," temporal keyword.**

Kling 3.0 supports native audio with lip-sync across 5 languages (Chinese, English, Japanese, Korean, Spanish) plus dialects. The syntax below is documented by fal.ai (infrastructure provider serving Kling) and echoed across practitioner sources.

## Core Format

```
[Character A: Role name, voice tone descriptor]: "Line of dialogue."
```

### Rules

1. Character label in brackets opens each line
2. Role name after the colon identifies who this is
3. Voice tone/emotion descriptor inside the bracket (comma-separated)
4. Language optional at end of bracket for multilingual
5. Dialogue line in double quotes
6. Character labels must be **identical** every time you reference the same character — no synonyms, no pronouns

## The Four Dialogue Principles (P1–P4) — from fal.ai

### P1 — Structured Naming

Character labels must be unique and consistent. Avoid pronouns or synonyms after introduction.

❌ **Wrong:**
```
[Character A: Woman]: "Let's go."
She smiled.
[Woman]: "I'm ready."
```

✅ **Right:**
```
[Character A: Woman]: "Let's go."
[Character A: Woman, soft tone]: "I'm ready."
```

### P2 — Visual Anchoring

Bind dialogue to a character's unique physical action. Describe the action first, then the dialogue line.

❌ **Wrong:**
```
[Character A: Lead Detective]: "Where is the truth?"
```

✅ **Right:**
```
The black-suited agent slams his hand on the table. Ceramic clinks sharply.
[Character A: Lead Detective, angrily shouting]: "Where is the truth?"
```

### P3 — Audio Details

Assign unique tone and emotion labels inside the character bracket for every line. Don't use the same descriptor twice if the emotion shifts.

❌ **Wrong:**
```
[Character A: Mom]: "Shoes on!"
[Character A: Mom]: "Help."
```

✅ **Right:**
```
[Character A: Mom, fast urgent voice]: "Shoes on!"
[Character A: Mom, exhausted voice]: "Help."
```

### P4 — Temporal Control

Use linking words like "Immediately," "Then," "Suddenly," "A moment later," to sequence speakers and prevent the model from merging speech lines.

❌ **Wrong:**
```
[Character A: Detective]: "Let's stop pretending."
[Character B: Suspect]: "I already told you everything."
```

✅ **Right:**
```
[Character A: Detective, controlled serious voice]: "Let's stop pretending."
Immediately, the suspect shifts in their chair, tense.
[Character B: Prime Suspect, sharp defensive voice]: "I already told you everything."
```

> [!test] Test first — "Immediately," keyword
> This temporal keyword is documented by fal.ai but not confirmed in first-party Kuaishou documentation. If you're building production workflows, verify behavior with a paired test (with and without the keyword) before committing.

## SFX Notation

For sound effects tied to actions:

```
SFX: A massive power-up sound effect like a turbine spinning at max speed that cuts the silence of the final frame
```

Or inline:

```
The detective stands suddenly, chair scraping back. SFX: Chair scrape, footsteps on wood.
```

## Full Dialogue Scene Template

```
[SCENE: location, atmosphere, ambient sound, light source]

[Character A: Role, physical description]
[Character B: Role, physical description]

[Setup action — who's visible, camera framing]
[Character A's physical anchor action]
[Character A: Role, tone]: "Line."

Immediately, [Character B's reaction]
[Character B: Role, tone]: "Response."

[Optional: environmental beat]
[Character A, different tone]: "Follow-up line."

[Optional: shot change for visual variety]
[Character B: Role, shifted tone]: "Closing line."

Audio: [ambient], [music swells if any], [SFX with timing]
```
