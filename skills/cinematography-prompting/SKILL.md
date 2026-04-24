---
name: cinematography-prompting
description: Use when writing any AI video prompt with vague cinematography language ("cinematic lighting", "camera moves", "she walks", "dramatic lighting") — substitutes named camera movements, named light sources, and physics-based motion descriptors so diffusion models (Kling 3.0, Seedance 2.0) render what you mean. Called by seedanceX, kling-video, shotlist as the substitution layer before final assembly.
---

# Cinematography Prompting

## Overview

A substitution layer that translates vague cinematography language into named anchor tokens recognized by video diffusion models. Video models were trained on cinematography-captioned footage — specifics beat generalities.

**Core principle:** Replace abstract phrasing with named sources from the training distribution. Never output "cinematic lighting", "camera moves", "dramatic light", or "she walks" as-is.

## When to Use

- A user's prompt contains vague camera / light / motion language
- A tool-specific skill (`seedanceX`, `kling-video`) is assembling a final prompt and needs a substitution pass
- Building a shot list and writing the first version of a shot description
- Reviewing a rendered clip that missed the camera / lighting intent (the prompt was probably vague)

**Do NOT use for:**
- Dialogue syntax → use `video-dialogue-prompting` (Kling 3.0 native-audio only)
- Platform-specific token limits, negative prompts, tool constraints → those live in the tool-specific skills
- Still-image prompting — Light Sources + Film Stocks transfer cleanly, but camera vocabulary is motion-specific

## Core Logic

1. **Scan the user's prompt for forbidden abstract tokens** (see `anti-patterns.md`).
2. **For each hit, substitute from the appropriate library:**
   - Camera → `camera-vocabulary.md`
   - Light → `light-sources.md` + color language table
   - Motion / action → `physics-translations.md` (Medium confidence — flag if uncertain)
3. **Anchor Hands rule:** If hands are in frame and unspecified, ASK what they're holding or touching before substituting. This is interactive, not a silent rewrite.
4. **Preserve confidence markers:** physics rules are `[Medium]`; the "tracking camera" magic token is `[Medium]`; the "Immediately," temporal keyword is `[Test first]`. Pass these through to the final prompt's notes section — do not flatten them into gospel.
5. **Assemble layers:** camera layer + lighting layer + subject + action (physics-translated) = cinematography-grade prompt.

## Quick Reference

| Abstract input | Substitute with |
|---|---|
| "camera moves" | Named camera move from `camera-vocabulary.md` (e.g., `slow dolly push`) |
| "cinematic lighting" | Named light source + direction (e.g., `golden hour sunlight catching dust particles`) |
| "dramatic lighting" | Specific physical source + color (e.g., `magenta neon reflecting off wet asphalt`) |
| "she walks" | `Each step lands heel-first, rolling forward through the ball of the foot` |
| "he picks up the cup" | `Fingers close around the ceramic, thumb resting on top, lifts from saucer with steady grip` |
| "warm lighting" | Named warm source (`low-hanging pendant lamps cast warm amber pools`) |
| "blue lighting" | `Cool blue haze filling the corridor` |

## Shot Assembly Formula

```
[Camera layer] + [Lighting layer] + [Subject] + [Action (physics-translated)]
```

Example:
> "Slow dolly push from MCU to ECU over 3 seconds. 85mm lens, shallow depth. Low-hanging pendant lamps cast warm amber pools. Her fingers close around the ceramic cup, thumb resting on top."

## The Anchor Hands Rule (interactive)

If hands appear in the prompt and the user hasn't specified what they're doing, STOP and ask what they're holding or touching. Do not silently rewrite. This is the single biggest hand-stability fix across diffusion models.

## Cross-Tool Applicability

| Layer | Kling 3.0 | Seedance 2.0 | Nano Banana (still) |
|---|---|---|---|
| Camera Vocabulary | High | Partial — Seedance reinterprets, avoid compound moves | N/A (still) |
| Light Sources | High | High | High |
| Physics Translations | Medium (verify with A/B) | Medium (verify) | N/A |
| Film Stocks | High | High | High |

## Reference files

- `camera-vocabulary.md` — full library (Push/Pull, Lateral, Vertical, Rotational, Handheld, Specialty + shot sizes + lens character)
- `light-sources.md` — 30+ named sources across Natural, Urban Night, Interior, Cinematic Practical + color language
- `physics-translations.md` — action → physics translations, Anchor Hands rule, domain vocabulary, self-test protocol
- `film-stocks.md` — 6 stock references for style anchoring
- `domain-vocabulary.md` — ballet / martial arts / cooking / piano / tennis / yoga / construction / coffee terminology
- `anti-patterns.md` — consolidated don't-do list

## Anti-Patterns

- Listing 5+ camera moves in one shot → pick the dominant one
- Mixing contradictory moves ("static camera with fast pan")
- Using "cinematic camera" as a substitute for specifics
- Abstract light terms alone ("dramatic", "moody", "beautiful")
- Free-floating hands — always anchor to an object
- Emotional abstractions for motion ("contemplates with heavy heart") — stage physically
- Technical specs the models ignore: fps, ISO, exact focal length (use lens buckets: wide / normal / telephoto)

## See Also

- [[video-dialogue-prompting]] — Kling-only native-audio dialogue syntax
- [[seedanceX]] (Overseer A) — Seedance-specific prompt assembly
- [[kling-video]] — Kling-specific prompt assembly
- [[shotlist]] — orchestrator that calls this skill per shot
- [[review-video]] — QA that audits rendered output against the substitutions
- [[2026-04-20-positive-framing-over-negative|Decision: Positive framing]]
