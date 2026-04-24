# Negative Prompts

**Confidence: [Medium] on the 5–7 item count rule, [High] on targeted-beats-exhaustive principle (architecture-general across diffusion models).**

## The Rule

Short, targeted negative prompts beat long exhaustive lists. Long negatives compete with each other for attention weight and can cause stiffness in the output. Target 5–7 items that address the specific failure modes likely in your scene.

## Baseline Negative Prompt

Use this as a starting point for any prompt:

```
sliding feet, distorted limbs, extra fingers, morphing textures, flickering, unnatural gait, blurry faces
```

## Scene-Specific Additions

Add 2–3 scene-specific negatives on top of the baseline, matched to likely failure modes:

### Walking / Running Scenes

```
sliding feet, moonwalk, floating motion, robotic gait, foot sliding
```

### Close-up Face Shots

```
face morphing, identity drift, asymmetric features, distorted eyes, warped mouth
```

### Hand-Focused Shots

```
extra fingers, merged fingers, floating hands, distorted knuckles, six fingers
```

### Multi-Character Scenes

```
character merging, identity swap between shots, face swap, doubled characters
```

### Product / Commercial Shots

```
warped text, unreadable typography, brand logos, distorted reflections, floating objects
```

### Realism / Cinema Look

```
cartoonish, 3D render, smooth plastic skin, smiling, video game aesthetic, animated
```

### Dialogue Scenes

```
lip-sync offset, identity drift between shots, mouth out of sync, silent speaking
```

### Dynamic Action Scenes

```
stiff movement, robotic motion, unnatural physics, floating combat, glitching
```

## Composition Patterns

### Pattern A: Inline (for API or UIs without separate negative field)

Append to the main prompt:

```
[Main prompt describing scene...]

No morphing clothes. No distorted hands. No flickering textures. Stable facial features. No sliding feet.
```

### Pattern B: Separate negative field (for APIs that support it)

```
Main prompt: [Scene description]
Negative prompt: sliding feet, distorted limbs, extra fingers, morphing textures, flickering, unnatural gait, blurry faces
```

### Pattern C: Targeted constraint phrasing

For specific technical constraints:

```
[Main prompt]

Camera constraints: No shake. Stable face. No morphing. No circular motion around object.
Motion constraints: Heel-to-toe contact, no floating, weight transfer visible.
```

## Common Mistakes

### Too Long (causes stiffness)

❌ "blur, flicker, distorted faces, warped limbs, unrealistic proportions, blurry textures, morphing, deformed hands, extra fingers, mutation, ugly, disfigured, low quality, artifacts, glitch, warped text, unreadable typography, bad anatomy, bad proportions, extra arms, extra legs, malformed, grotesque, horror, scary, distorted face, watermarks, signature, username, error, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry, artist name"

### Too Vague

❌ "bad stuff, low quality, ugly"

### Contradictory

❌ "no motion blur, no static shot" (one implies movement, the other implies stillness)

### Using Positive Framing

❌ "sharp image, clear face" (these should be in the main prompt)

## The Skill's Auto-Generation Logic

When generating a negative prompt:

1. Start with the baseline (7 items)
2. Add the scene-type additions (2–3 items) matching the scene
3. If user hints at a specific concern (hands, dialogue, etc.), prioritize that category
4. Cap at 10 items maximum
5. Output as comma-separated list

## Self-Test Protocol

Validate the "short beats long" rule yourself:

1. Generate the same scene three times:
   - A: No negative prompt
   - B: 5-item targeted negative
   - C: 20-item exhaustive negative
2. Review for: motion naturalness, body proportions, subject behavior
3. If B clearly outperforms C, the rule holds for your use case
4. If A ties with B, negatives may not be providing value at your complexity level

## Failure Mode: Negative Prompt Conflict

If the user's main prompt contains something the negative tries to exclude, the model will struggle:

- Main: "A cartoonish animated character walks..."
- Negative: "cartoonish, animated"

Always check the main prompt for conflicts before emitting negatives.
