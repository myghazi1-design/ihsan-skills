---
type: skill-reference
skill: seedanceX
topic: negatives
---

# Seedance Negatives — Positive Framing + 3-Max Rule

## Core Principle

Describe what you WANT, not what to avoid. Reserve negatives (max 3) for the END of the prompt. Only use negatives for things that CAN'T be stated positively.

## Positive-Framing Substitution Table

| Instead of (negative) | Use (positive) |
|----------------------|----------------|
| "avoid floating hands" | "anatomically correct hands" |
| "avoid bent limbs" | "physically accurate motion" |
| "avoid jitter" | "smooth stable camera" |
| "no distorted hands" | "fingers gripping the object" |
| "avoid identity drift" | "consistent facial features throughout" |
| "avoid chaotic composition" | "clean composition with one clear focal point" |

## Allowed Negatives (end of prompt, max 3)

For things that genuinely can't be stated positively:

- `No text, no watermark`
- `No stylization, no game look`
- `No music`

## Optional Safety Negatives (when relevant)

From the earlier research baseline — use ONLY if positive framing failed on a test render:

- `avoid jitter` (any shot)
- `avoid bent limbs` (any shot with people)
- `avoid temporal flicker` (videos over 10s)
- `avoid identity drift` (any character content)
- `avoid chaotic composition` (complex scenes)

**Prefer the positive-framing column above.** The "avoid X" list is fallback.

## Directive-No vs Avoid-No

The POV/Orbs required constraint is a DIRECTIVE negative, not an avoidance:

```
No cuts, no zoom, natural head movement.
```

This is an instruction about camera behavior, not a "avoid distorted hands"-style failure mode. It stays at the POV/Orbs format tail regardless of the 3-max rule.

## Failure Modes of Bad Negatives

- **Too long (10+ items)** → causes stiffness in output
- **Vague** → "bad stuff, low quality, ugly" adds nothing
- **Contradictory** → "no motion blur, no static shot" — one implies motion, one implies stillness
- **Positive framing in negatives** → "sharp image, clear face" belongs in main prompt
- **Main-prompt conflict** → if main says "cartoonish", negative "no cartoon" is a double-bind

## Auto-Generation Logic

1. Default: 0 negatives (positive framing only)
2. If the shot is character-heavy: add up to 2 targeted from the avoid-list
3. If dialogue at seam: add `lip-sync offset`
4. Cap at 3
5. Always end with `No music.` for Mode 1 / Mode 2 clips (music is post-production)
