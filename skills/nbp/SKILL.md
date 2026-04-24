---
name: nbp
description: Use when the user asks to generate a Nano Banana Pro 4K reference image, a starting/reset frame for Seedance or Kling video, a character/setting/product still for AI video pipelines, or mentions "NBP", "Nano Banana Pro", "AI.Studio image", "4K reference image", "start frame", or "JSON method" for image generation. Routes to NBPHero for product-hero and label-transcription work.
version: 1.0.0
tested_on: Nano Banana Pro via AI.Studio (April 2026)
---

# Nano Banana Pro (NBP) Image Builder

Produce the 4K starting/reset reference image that carries character appearance downstream into Seedance or Kling. NBP images are the foundation of the AI video pipeline — the image carries the character's appearance, so video prompts only have to handle motion + dialogue + tone.

## When to Use

Activate when the user:
- Says "generate the image for the video", "start frame", "4K reference image", "NBP", "Nano Banana Pro"
- Needs a character/setting/product still as a starting asset for Seedance or Kling
- Asks about the "JSON method" (Pinterest → Gemini 3.0 Pro JSON extraction → NBP)
- Asks to do a background swap while preserving character identity

Route AWAY to `NBPHero` when:
- The request involves a product hero shot, label transcription, or a locked-in kit arrangement
- The request is a flat-lay of a product kit

Do NOT activate for:
- Video generation → `seedanceX` or `kling-video`
- Voice generation → `voice-elevenlabs`

## Core Principle

NBP images are the foundation of the AI video pipeline. **The reference image carries the character's appearance** — video prompts should not re-describe what NBP already nailed. Get it right here and everything downstream is easier.

## How to Use

### Step 1 — Confirm aspect ratio BEFORE generating

Lock aspect first. The platform target drives this:

| Platform | Aspect |
|---|---|
| YouTube, TV | 16:9 |
| TikTok / IG Reels / YouTube Shorts | 9:16 |
| Instagram feed | 1:1 |
| Portrait-heavy Instagram | 4:5 |

### Step 2 — Assemble prompt in order

```
[brief subject description] + [detailed specific elements] + [modifiers] + [framing] + [style]
```

See `references/prompt-structure.md` for the full element breakdown.

### Step 3 — Use JSON Method for aesthetic precision

The JSON Method is 10× better than text-only prompts. Use when the aesthetic / grading / realism matters:

1. **Find a reference image on Pinterest** — search for the aesthetic you want
2. **Convert to JSON** — upload to Gemini 3.0 Pro and ask: `"Create a detailed JSON to recreate this image."` Copy the JSON output.
3. **Use JSON as base** — paste JSON + prompt. Example: `"Use this JSON as reference: [JSON] Now generate: woman holding my product on beach."`
4. **Generate in AI.Studio** — paste the JSON + prompt and generate.

**Why this works:** The JSON handles realism + color grading. Your prompt handles the actual content. This prevents the standard grey AI look from text-only prompts.

**Alternative source:** TikTok screenshots of niche-performing frames → same JSON extraction flow.

### Step 4 — Character preservation (background swap)

When swapping backgrounds while keeping a character identical, lead with:

```
[reference person 1] — preserve all facial features, hair color and style,
skin tone, body type, and physical characteristics of the reference photo 1
exactly as they appear
```

Then describe the new scene/background AFTER this line.

### Step 5 — Apply best practices

- **Always "4K" in the prompt** — these are start frames for video, resolution matters
- **Match aspect ratio to platform** — decide before generating, not after
- **One subject per frame** — clean for Kling/Seedance to interpret
- **Nail appearance here** — video prompts handle only motion/dialogue/tone
- **Text on products** — use `perfect text` and `legible text` modifiers. Still images render these well; video models smudge regardless.
- **Lighting in the image sets the mood** — Kling/Seedance will largely inherit lighting from the start frame

### Step 6 — Content-type adjustments

- **AI Podcast / Talking Head** — direct-to-camera, clean background, eye contact with lens
- **UGC Product Review** — subject holding/interacting with product, casual setting, use `perfect text` for brand
- **Military / Action** — get tactical gear right at NBP stage (NODs / GPNVGs / plate carriers / weapon systems / IR devices). Seedance handles motion, NBP handles accuracy.

### Step 7 — Output

Return:
1. The final NBP prompt (ready to paste into AI.Studio)
2. The aspect ratio specifier
3. Any preservation / modifier lines required
4. A note on which downstream video skill will consume this (Seedance or Kling)

## Element Breakdown

| Element | Purpose | Examples |
|---|---|---|
| **Subject** | Who/what is in the frame | "a woman in her 30s sitting on a couch", "a Navy SEAL in full kit" |
| **Specific details** | Clothing, gear, environment, props | "wearing a black hoodie, holding a product box, modern living room background" |
| **Modifiers** | Quality and text control | `perfect text`, `legible text`, `highly detailed`, `sharp focus` |
| **Framing** | Aspect ratio for target platform | `16:9`, `9:16`, `1:1`, `4:5` |
| **Style** | Visual aesthetic | `photorealistic`, `cinematic`, `film grain`, `vintage`, `studio lighting` |

## Platform-Specific Templates

### TikTok / YouTube Shorts / Meta Reels (9:16)
```
[subject description], [specific details], [modifiers], 9:16 vertical framing, [style], 4K
```

### YouTube Longform (16:9)
```
[subject description], [specific details], [modifiers], 16:9 widescreen framing, [style], 4K
```

### Square (1:1)
```
[subject description], [specific details], [modifiers], 1:1 square framing, [style], 4K
```

## Reference Files

Living in `Skills/nbp/`:
- `notes.md` — full reference with JSON method, character preservation, content types
- `prompt-structure.md` — element breakdown with more examples
- `platform-templates.md` — 9:16, 16:9, 1:1, 4:5 variants

## See also

- `NBPHero` — product-hero subclass (labels, flat-lay kits, text-rendering ceiling)
- `seedanceX` — downstream consumer of the NBP start frame (action / cinematic / UGC)
- `kling-video` — downstream consumer for talking-head / static / dialogue content
- `shotlist` (Overseer B) — orchestrator that requests NBP images at Clip 1 and reset points
