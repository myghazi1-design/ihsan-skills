---
name: video-dialogue-prompting
description: "Use when writing Kling 3.0 video prompts with on-screen dialogue, lip-sync, character conversation, interrogation scenes, multilingual speech, or native-audio output. Enforces the fal.ai P1-P4 principles (structured naming, visual anchoring, audio details, temporal control) and the [Character A: Role, tone]: \"Line.\" syntax so speech doesn't merge, identities don't drift, and lip-sync holds. Kling-only — Seedance 2.0, Mikos, Nano Banana do not ship native audio today."
---

# Video Dialogue Prompting

## Overview

Kling 3.0's native audio + lip-sync feature requires a specific character-label syntax to avoid merged speech, identity drift, and floating voices. This skill enforces the fal.ai-documented P1–P4 principles as the default assembly pattern.

**Kling-only today.** Seedance 2.0, Mikos, and Nano Banana do not ship native audio — do NOT emit this syntax in prompts routed to those models.

## When to Use

- Prompt contains on-screen speech, lip-sync, character conversation
- Interrogation, argument, Q&A, podcast, multilingual scene
- User asks for "dialogue", "lip-sync", "two people talking", "voice over that syncs"
- A [[shotlist]] routes a shot to Kling 3.0 with dialogue

**Do NOT use for:**
- Voice-over B-roll (no mouth on camera) — that's standard Kling or Seedance
- Seedance / Mikos / Nano Banana prompts (no native audio)
- Audio-only generation — use [[voice-elevenlabs]] (Overseer A) instead

## Core Format

```
[Character A: Role name, voice tone descriptor]: "Line of dialogue."
```

## The Four Principles (P1 – P4) — enforce every time

### P1 — Structured Naming
Character labels must be unique and consistent. Reuse the **exact label** — no pronouns, no synonyms after introduction.

### P2 — Visual Anchoring
Bind dialogue to a unique physical action described BEFORE the line.

### P3 — Audio Details
Fresh tone/emotion descriptor per line when emotion shifts. Never repeat the same descriptor across emotional beats.

### P4 — Temporal Control
Temporal keyword (`Immediately,` · `Then,` · `Suddenly,` · `A moment later,` · `After a beat,`) between speaker changes. Prevents the model from merging speech lines.

> [!test] Test first — "Immediately,"
> Documented by fal.ai, not confirmed in first-party Kuaishou docs. Pair-test with and without before committing for production.

## Full Scene Assembly

```
[SCENE: location, atmosphere, ambient sound, light source]

[Character A: Role, physical description]
[Character B: Role, physical description]

[Setup action — who's visible, camera framing]
[Character A's physical anchor action]
[Character A: Role, tone]: "Line."

Immediately, [Character B's physical reaction]
[Character B: Role, tone]: "Response."

[Optional environmental beat]
[Character A, SHIFTED tone]: "Follow-up line."

Audio: [ambient], [music if any], [SFX with timing]
```

## Quick Reference — Tone Descriptors

**Emotional states:** controlled serious voice · warm nostalgic voice · shouting loudly · voice cracking · sharp defensive voice · calm but threatening tone · exhausted voice · fast urgent voice · hesitant voice · soft tone · whispering · trembling frustrated voice · confident authoritative voice · sleepy amused voice · annoyed sarcastic tone · emotional voice · cold quiet tone

**Physical voice qualities:** raspy, deep voice · clear fearful voice · breathy · booming · thin high voice · gravelly · smoky · crisp articulate · mumbling · rapid-fire

**Temporal keywords:** Immediately, · Then, · Suddenly, · A moment later, · Pause. · Silence. · After a beat,

## Multilingual

Append language after tone inside the bracket:

```
[Character A: Woman, soft tone, French]: "Tu crois que c'est possible?"
[Character B: Man, thoughtful tone, English]: "I think we have to try."
```

Supported in Kling 3.0: Chinese (+ dialects), English, Japanese, Korean, Spanish.

## Negative prompts for dialogue

Add to Kling's negative field: `lip-sync offset, identity drift between shots, mouth out of sync, silent speaking`.

## Reference files

- `syntax-core.md` — format spec + P1–P4 principles verbatim (fal.ai reference)
- `temporal-keywords.md` — keyword library + usage rules
- `tone-descriptors.md` — emotional + physical voice qualities
- `multilingual.md` — language append pattern + supported list
- `examples.md` — verbatim interrogation + Tokyo night market scenes from source

## Common Mistakes

| Mistake | Why it fails | Fix |
|---|---|---|
| Using "Woman" then "she" then "the woman" for same character | P1 violation — Kling can't track identity across synonyms | Reuse the exact label `[Character A: Woman]` every time |
| Line without physical anchor | P2 violation — floating voice | Describe an action before the line ("She slams her palm on the table.") |
| Same tone descriptor across emotional beats | P3 violation — model flattens emotion | Shift descriptor every time emotion changes |
| No temporal keyword between speakers | P4 violation — speech merges | Add `Immediately,` / `Then,` / `Suddenly,` between lines |
| Mouth on camera at clip seam (chained Seedance) | Lip-sync fails at boundaries | Keep all dialogue in a single Kling clip, not across a chain |

## See Also

- [[cinematography-prompting]] — scene / camera / lighting layer (call this first, then layer dialogue on top)
- [[kling-video]] — parent tool skill (dialogue mode)
- [[shotlist]] — routes talking-head shots to Kling + this skill
- [[ugc-creator-setup]] — UGC scripts use dialogue mode for talking-head video type
- [[review-video]] — QA audits lip-sync offset and identity drift
