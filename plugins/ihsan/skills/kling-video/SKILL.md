---
name: kling-video
description: Use when the user asks to write, generate, improve, or audit a Kling 3.0 (Kuaishou) AI video prompt — especially for talking-head dialogue, static / locked-camera shots, lip-sync precision, 3-character scenes, small-object detail, literal-camera-instruction shots, or any mention of kling.ai / kling-v3 / Kling Omni / Kling Motion Control. Also use for multi-shot storyboards up to 6 shots, native-audio dialogue format, or image-to-video with preserve-then-animate logic.
version: 2.0.0
tested_on: Kling 3.0 (April 2026)
---

# Kling 3.0 Video Prompt Generator

Auto-generate production-quality Kling 3.0 prompts. The skill enforces the Five-Layer Formula by construction, substitutes vague language for anchor tokens the model recognizes, and validates platform constraints. Kling prompts are SHOT INSTRUCTIONS (Kling follows them literally), unlike Seedance prompts which are scene descriptions.

## When to Use

Activate when the user:
- Asks to write a Kling 3.0 prompt ("write me a Kling prompt for...")
- Provides a video concept that requires: static camera, literal camera instructions, lip-sync dialogue, 3+ characters, or small-object detail
- Asks about Kling 3.0 syntax, dialogue format, or multi-shot structure
- Mentions Kling AI, Kuaishou, kling.ai, kling-v3, kling-v3-omni, Kling Motion Control

Route AWAY to `seedanceX` instead when:
- The shot is action-heavy / wide / environmental / creature / UGC-timestamp — Seedance handles those better
- User wants Seedance-specific formats (Transformations, Orbs, POV with VFX)

Do NOT activate for:
- Other video models (Sora, Veo, Pika, Runway) — their prompt formats differ
- Still image generation → `nbp` or `NBPHero`
- Video editing or post-production questions

## How to Use

### Step 1 — Classify the prompt type

From user intent, determine which mode to use:

| Mode | Trigger | Output |
|---|---|---|
| **Quick** | Simple 5s clip, no dialogue | Layers 1-4 + negatives |
| **Full** | Narrative scene, 10-15s | All 5 layers + negatives |
| **Dialogue** | Spoken lines mentioned | All layers + `[Character: Role, tone]:` format |
| **Multi-shot** | "multiple shots", "storyboard", "sequence" | Master Prompt + numbered shots (max 6) |
| **Image-to-video** | User provides source image | Preserve-then-animate format |

### Step 2 — Parse user intent into the five layers

Extract or ask for each layer. If the user gives a vague description, ask targeted questions — one per missing layer, batched in a single turn.

| Layer | What to extract |
|---|---|
| **Scene** | Location, time of day, atmosphere, named light sources |
| **Characters** | Identity, physical description, clothing, objects they interact with |
| **Action** | Physical mechanics, temporal arc (beginning → middle → end) |
| **Camera** | Shot size, movement, angle, style |
| **Audio** | Dialogue (if any), ambient sounds, music, SFX |

### Step 3 — Apply the enforcement rules

See `references/enforcement-rules.md` for the full list. The critical rules:

1. **Named light sources only** — Reject "dramatic lighting", "cinematic lighting", "moody lighting". Substitute from `references/light-sources.md`.
2. **Named camera movements only** — Reject "camera moves closer", "camera follows". Substitute from `references/camera-vocabulary.md`.
3. **Physics-over-action for motion** — Translate action verbs into physical mechanics. See `references/physics-translations.md`. Flag as `[Test first]` if user hasn't validated on their setup.
4. **Anchor hands to objects** — If hands appear in the shot, they must be doing something to an object. If unspecified, ASK what they're holding.
5. **Character label consistency** — For dialogue or multi-character: `[Character A: Role, tone]:` format throughout, identical labels across all references.
6. **Multi-shot cap at 6** `[Verified]` — Hard constraint. Refuse 7+ shots; suggest splitting.
7. **Max duration 15s** `[Verified]` — Sum shot durations. If >15s, scale down or remove shots.
8. **Negative prompt size 5-10** — Cap at 10 to prevent stiffness.

### Step 4 — Output the prompt using the right template

See `references/prompt-template.md` for all 5 template shapes. Choose by mode.

### Step 5 — Append scene-appropriate negatives

Start with the 7-item baseline, add 2-3 scene-specific items, cap at 10. See `references/negative-prompts.md`.

Baseline (7 items):
```
sliding feet, distorted limbs, extra fingers, morphing textures, flickering, unnatural gait, blurry faces
```

### Step 6 — Emit output + Notes

Include a "Notes" section identifying:
- Which rules were applied
- Confidence tier for any `[Medium]` or `[Test first]` claims
- A suggestion to run the self-test protocol if it's the user's first use

## Confidence Tiering

Every claim in this skill is tagged. Propagate the relevant tier labels where the user would benefit from knowing confidence:

- `[Verified]` — First-party kling.ai documentation. Platform constraints (6-shot, 15s, pricing, model IDs).
- `[High]` — Mechanistic backing + convergence across production sources.
- `[Medium]` — Single-source practitioner observation, mechanistically plausible (e.g., physics-over-action).
- `[Test first]` — Worth validating on user's own setup (e.g., "Immediately," temporal keyword, "tracking camera" magic token).

## Dialogue Mode — native audio + lip sync

Core format (verbatim from fal.ai, [High]):
```
[Character A: Role name, voice tone descriptor]: "Line of dialogue."
```

Four Dialogue Principles (P1-P4):
- **P1 Structured Naming** — unique, consistent character labels; no pronouns
- **P2 Visual Anchoring** — bind each line to a physical action described BEFORE the line
- **P3 Audio Details** — fresh tone descriptor per line when emotion shifts
- **P4 Temporal Control** — linking words ("Immediately,", "Then,", "Suddenly,") between speakers

Temporal keywords are `[Test first]` — documented by fal.ai but not first-party confirmed. See `references/dialogue-syntax.md`.

## Multi-Shot Mode — max 6 shots, ≤15s total

```
Master Prompt: [Overall narrative intent — one sentence]

Shot 1 (0-Xs): [Framing] [Camera] [Action] [Lighting]
Shot 2 (X-Ys): ...
[max 6 shots]

Audio: [ambient for whole sequence], [music], [SFX with timing]

Negative: [5-7 targeted items]
```

Use `@Character` references for identity consistency across shots: `@Male lead`, `@Female lead`.

## Image-to-Video Mode — preserve THEN animate

```
Animate with [CAMERA MOVEMENT]. Preserve [IDENTITY/LAYOUT/TEXT from source].

[WHAT MOVES]:
- Primary motion: [subject action]
- Secondary motion: [fabric, hair, steam]
- Environmental motion: [clouds, water, light]

[WHAT STAYS STATIC]:
- [Elements to preserve exactly from source]

Negative: style drift, morphing [preserved elements], identity change
```

Subtle motion usually beats dramatic motion in I2V.

## Reference Files

The skill's knowledge base lives in `Skills/kling-video/references/`:

- `enforcement-rules.md` — 8 Hard Rules + 4 Soft Rules + refusal conditions + substitution table
- `prompt-template.md` — 5 fill-in templates (Quick / Full / Dialogue / Multi-Shot / I2V) + output checklist
- `platform-specs.md` — Model variants, hard capabilities, pricing, v3-vs-v2.6 diff, version history
- `negative-prompts.md` — Baseline + 8 scene-type additions + 3 composition patterns
- `light-sources.md` — Named light-source library (natural / urban night / interior / cinematic practical) + color language + film stocks
- `camera-vocabulary.md` — 28-verb camera-move library + shot sizes + lens character + anti-patterns
- `physics-translations.md` — Action-verb → physics translations + Anchor Hands rule + domain vocab
- `dialogue-syntax.md` — P1-P4 principles + temporal keywords + tone descriptors + multilingual + SFX
- `verified-examples.md` — 14 tested prompts across 6 categories (Product / B-Roll / Human Motion / Multi-Shot / Dialogue / I2V)
- `production-prompts/` — 66 verbatim reference prompts from cliprise + imagine.art

## Failure Modes to Watch For

1. **User insists on "dramatic lighting"** — Explain named sources produce better results; offer 2-3 alternatives. Don't override without explanation.
2. **User wants more than 6 shots** — Refuse with explanation. Suggest splitting into two generations.
3. **User describes action without physics** — Translate in output, flag as `[Test first]` if for human motion.
4. **User provides an existing prompt to "fix"** — Parse into 5 layers, identify weak/missing ones, rewrite applying rules.
5. **User asks "does Kling support X"** — Check `references/platform-specs.md` before answering. Don't guess.
6. **Kling model update (3.5 / 4.0)** — Flag that templates are version-pinned to 3.0 and may need re-testing.

## Cross-Skill Combos

- **Route to `seedanceX`** when shot is action / wide / UGC-timestamp / creature / environmental
- **`nbp` upstream** for I2V source image generation (4K reference frames)
- **`NBPHero` upstream** for product-hero I2V (label transcription, flat-lay)
- **`voice-elevenlabs`** for external VO pre-gen (optional — Kling has native audio, but ElevenLabs beats it on quality)
- **`review-video` (Overseer B)** downstream for QA
- **`shotlist` (Overseer B)** calls this per-shot for dialogue/static/lip-sync shots

## See also

The existing reference file at `Skills/kling-video/SKILL.md` is the older locator/guide. This new executable SKILL.md is the canonical entry point for the Kling 3.0 prompt builder.
