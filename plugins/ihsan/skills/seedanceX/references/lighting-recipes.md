---
type: skill-reference
skill: seedanceX
topic: lighting-recipes
---

# Seedance Lighting Recipes

**Highest leverage single element.** If you can only add one detail, add lighting.

## Named Lighting Recipes

| Type | Phrase |
|------|--------|
| Golden hour | "warm golden hour sunlight" |
| Overcast | "soft diffused cloud light, no harsh shadows" |
| Blue hour | "blue twilight, cool tones" |
| Studio | "soft box light from the left, fill from the right" |
| Hard contrast | "hard chiaroscuro, deep shadows" |
| Neon/urban | "neon reflections on wet pavement" |
| Window | "soft natural window light from the right" |
| Night ops | "faint window spill, moonlight, no artificial light" |
| Harsh sun | "harsh overhead midday sun, visible heat haze" |

## Rule

Always name the **source** and **direction**. Example:

- Bad: "dramatic lighting"
- Good: "Soft key light from 45 degrees left, warm rim light"

## Temperature Language

Use fixed values, not ranges:

- Bad: "warm-ish lighting"
- Good: "warm 3200K key light"

Prevents cross-clip drift when chaining.

## Repeat per Segment

In Mode 2 multi-segment prompts and chains, repeat the lighting descriptors in EVERY segment. Don't assume carry-forward.

## Substitution Rules

Refuse these and substitute:

| ❌ Abstract | ✅ Specific |
|---|---|
| "dramatic lighting" | "single pendant lamp from above, rest in shadow" |
| "cinematic lighting" | "golden hour backlighting" or spell out the recipe |
| "warm lighting" | "golden hour sunlight" or "warm pendant lamp" |
| "cool lighting" | "moonlight" or "LED panel lighting" |
| "blue lighting" | "cool blue haze filling the corridor" |
| "red lighting" | "magenta neon reflecting off wet asphalt" |
| "green lighting" | "flickering fluorescent tubes with sick green tint" |
| "teal lighting" | "teal and orange color grade, crushed blacks" |

## Film Stock Adjuncts

When a specific look is wanted:

- `35mm film, shallow focus` — warm, grainy, shallow depth
- `Super 8 film` — vintage warm tones, heavy grain
- `VHS camcorder` — heavy grain, chromatic aberration, low fidelity
- `Digital cinema, anamorphic` — modern, sharp, horizontal lens flares
- `Kodak Portra` — flattering skin tones, warm neutral palette
- `Kodak Vision3 250D` — daylight balanced, cinematic contrast
