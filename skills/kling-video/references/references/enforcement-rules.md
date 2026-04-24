# Enforcement Rules

The skill enforces these rules by construction. Every generated prompt must pass this checklist before being returned to the user.

## Hard Rules (always enforce)

### 1. Layer Order — Scene → Characters → Action → Camera → Audio

**[High]** The Five-Layer Formula must be honored. Even in quick mode, the scene comes before the character, which comes before the action, which comes before camera, which comes before audio.

Validation: Read your generated prompt back. If the first sentence describes a person before establishing location, restructure.

### 2. Multi-Shot Cap at 6

**[Verified]** Kling 3.0 platform constraint. Hard limit.

Validation: Count shot labels (`Shot 1`, `Shot 2`, ...). If > 6, refuse and suggest splitting into two generations.

### 3. Max Duration 15 seconds

**[Verified]** Kling 3.0 platform constraint.

Validation: Sum all shot durations. If > 15s, scale down or remove shots.

### 4. Named Light Sources Only

**[High]** Never emit vague lighting terms. Substitute from `light-sources.md`.

Validation: Grep the output for these strings — if any are present, rewrite:
- "dramatic lighting"
- "cinematic lighting"
- "moody lighting"
- "beautiful lighting"
- "atmospheric lighting"
- "good lighting"

### 5. Named Camera Movements Only

**[High]** Never emit "camera moves" or similar generic phrasing. Substitute from `camera-vocabulary.md`.

Validation: If the prompt contains a camera verb, it must be one of:
- dolly push, crash push, dolly pull back, slow zoom, crash zoom
- tracking shot, whip pan, slow pan, parallel tracking
- crane up, crane down, tilt up, tilt down
- orbit, arc, rotation
- handheld, shoulder-cam drift, steadicam, gimbal stabilized
- POV, over-the-shoulder, rack focus, snap focus, speed ramp

### 6. Character Label Consistency for Dialogue/Multi-Character

**[High]** If multiple characters appear, use `[Character A: Role]` format consistently. The label string must be identical across all references.

Validation: Collect all character labels in the prompt. Each unique character should have exactly one label string that matches verbatim across all references.

### 7. Negative Prompt Size 5–10 items

**[Medium]** Baseline 7 items, can expand to 10 for complex scenes. Never more than 10.

Validation: Split negative prompt by commas. Count items. If < 5, add scene-specific additions. If > 10, trim to most relevant.

### 8. Hands Anchored When Visible

**[High]** If hands are in the shot, they must be doing something to an object, not moving freely.

Validation: If the prompt mentions hands, grep for:
- verb + object ("grips the cup", "rests on the railing", "taps the screen")
- If only "moves hands" or "gestures" appears without an object, ask for one.

## Soft Rules (apply and flag confidence)

### 9. Physics-Over-Action Translation

**[Medium — Test first]** For human motion shots, translate action verbs into physical mechanics. See `physics-translations.md`.

Apply: When the action layer describes human movement that will be visible, add physics descriptors.

Flag: Label as `[Test first]` in the output notes so the user knows this is a single-source recommendation.

### 10. Temporal Arc for Actions > 5 seconds

**[High]** For actions lasting more than 5 seconds, describe beginning/middle/end.

Apply: Break the action into 2–3 phases with temporal markers.

### 11. Texture Details for Realism

**[High]** Include 1–2 tactile details per scene (grain, condensation, fabric sheen, dust, smoke).

Apply: Add a texture element to the scene layer.

### 12. Temporal Keywords for Multi-Speaker Dialogue

**[Test first]** Use "Immediately," "Then," "Suddenly," between speakers.

Apply: Insert between dialogue lines.

Flag: Label as `[Test first]` — documented by fal.ai but not first-party confirmed.

## Substitution Table

Quick reference for the skill's automatic substitutions:

| User writes | Skill substitutes |
|---|---|
| "dramatic lighting" | Ask for specific source, or default to "single pendant lamp from above, rest in shadow" |
| "cinematic lighting" | Ask for specific source, or default to "golden hour backlighting" |
| "warm lighting" | "golden hour sunlight" or "warm pendant lamp" |
| "cool lighting" | "moonlight" or "LED panel lighting" |
| "camera moves closer" | "slow dolly push from medium shot to close-up over 3 seconds" |
| "camera follows" | "tracking shot" or "handheld shoulder-cam drift" |
| "camera zooms" | "dolly push" (for parallax) or "slow zoom" (for optical) |
| "walks" | "Each step lands heel-first, weight rolling forward through the ball of the foot" [Test first] |
| "runs" | "Full stride extension, feet striking ground with visible impact, arms pumping" [Test first] |
| "gestures" | Ask for anchor object; or "His hand rests on the [object], thumb tapping once" |
| "he smiles" | "Corners of his mouth curve upward, cheeks lift slightly, eyes crinkle at the edges" |

## Refusal Conditions

Refuse to generate and explain why:

1. **> 6 shots requested** — Suggest splitting into two generations
2. **> 15 second total duration** — Scale down or remove shots
3. **Character label collision** — Two different characters with the same label
4. **Contradictory negatives** — Main prompt contains something the negative excludes
5. **Missing required layer in full mode** — Ask for the missing layer
6. **Dialogue without audio mode enabled** — Warn that dialogue requires audio-capable API call

## Confidence Tier Propagation

When outputting the prompt, include a "Notes" section at the end that lists:
1. Which rules were applied
2. The confidence tier for any rule marked `[Medium]` or `[Test first]`
3. A suggestion to run the self-test protocol if it's the user's first use

Example:

```
Notes:
- Five-Layer Formula applied [High]
- Named light sources: "flickering neon signs" [High]
- Physics-over-action: "heel-first, weight rolling forward" [Medium — test first]
- "Immediately," sequencing: used between speakers [Test first]
- Negative prompt: 7 items, targeted [Medium]
```
