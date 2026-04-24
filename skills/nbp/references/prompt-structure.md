---
type: skill-reference
skill: nbp
topic: prompt-structure
---

# NBP Prompt Structure — Element Breakdown

## The Assembly Order

Build the prompt in this specific order. Order matters — NBP parses subject first.

```
[brief subject description] + [detailed specific elements] + [modifiers] + [framing] + [style]
```

## Detailed Element Guide

### 1. Subject (the WHO or WHAT)

Lead with a crisp identifying noun phrase.

| Good | Bad |
|---|---|
| "a woman in her 30s sitting on a couch" | "a person" |
| "a Navy SEAL in full kit" | "a soldier" |
| "a pill bottle on marble countertop" | "a bottle" |
| "a golden retriever on a beach at sunset" | "a dog" |

### 2. Specific Details (clothing, gear, environment, props)

Add the visible concrete details that matter. Skip what doesn't matter.

Examples:
- "wearing a black hoodie, holding a product box, modern living room background"
- "full plate carrier with GPNVGs, desert fatigues, sand-dusted boots"
- "cream label with 'daily glow' wordmark in white, orange 60 badge"

### 3. Modifiers (quality + text control)

- `perfect text` — when readable text on product matters
- `legible text` — softer version of the above
- `highly detailed`
- `sharp focus`
- `shallow depth of field`
- `deep focus`
- `natural skin texture`
- `film grain`

### 4. Framing (aspect ratio, always explicit)

- `16:9 widescreen framing` — YouTube longform
- `9:16 vertical framing` — TikTok, Reels, Shorts
- `1:1 square framing` — Instagram feed
- `4:5 portrait framing` — Instagram tall
- `21:9 anamorphic framing` — cinematic ultrawide

### 5. Style (visual aesthetic)

- `photorealistic`
- `cinematic`
- `film grain`
- `vintage`
- `studio lighting`
- `natural window light`
- `editorial fashion`
- `product photography`
- `documentary`

Always end with `, 4K`.

## Example Assembled Prompts

### Talking head (Kling downstream)
```
A woman in her 30s, dark hair pulled back, navy linen blazer, sitting
on a modern grey sofa in a bright home office with a neutral palette,
natural window light from the left, direct eye contact with camera,
photorealistic, shallow depth of field, 16:9 widescreen framing, 4K
```

### UGC product review (Seedance downstream)
```
A woman in her late 20s, casual style, holding a cream-colored pill
bottle toward camera with visible "daily glow" wordmark, perfect text,
seated on a linen couch with morning sunlight from a large window,
natural skin texture, photorealistic, 9:16 vertical framing, 4K
```

### Military / action (Seedance downstream)
```
A Navy SEAL in full night-ops kit, GPNVGs mounted on helmet, plate
carrier with IR strobe, desert MultiCam fatigues, sand-dusted boots,
in the open door of a blacked-out helicopter at night, moonlight with
no artificial light, cinematic, film grain, 16:9 widescreen framing, 4K
```

### Product hero flat-lay (route to NBPHero)
```
→ Route this to NBPHero. See Skills/NBPHero/notes.md.
```
