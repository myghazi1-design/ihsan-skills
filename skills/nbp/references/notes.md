---
type: skill-reference
skill: nbp
domain: video-production
tags: [nbp, prompting, image-generation, video-production]
---

# Nano Banana Pro Prompting

**Platform:** Nano Banana Pro (best via AI.Studio)
**Primary use case:** 4K reference images for AI video production (start frames for Kling 3.0 and Seedance 2.0)

---

## Core Principle

NBP images are the foundation of the AI video pipeline. The reference image carries the character's appearance — video prompts should not re-describe what NBP already nailed. Get it right here and everything downstream is easier.

## Prompt Structure

Follow this order:

```
[brief subject description] + [detailed specific elements] + [modifiers] + [framing] + [style]
```

### Elements Breakdown

| Element | Purpose | Examples |
|---------|---------|---------|
| **Subject** | Who/what is in the frame | "a woman in her 30s sitting on a couch", "a Navy SEAL in full kit" |
| **Specific details** | Clothing, gear, environment, props | "wearing a black hoodie, holding a product box, modern living room background" |
| **Modifiers** | Quality and text control | "perfect text", "legible text", "highly detailed", "sharp focus" |
| **Framing** | Aspect ratio for target platform | "16:9" (YouTube), "9:16" (TikTok/Shorts/Reels), "1:1" (square) |
| **Style** | Visual aesthetic | "photorealistic", "cinematic", "film grain", "vintage", "studio lighting" |

## The JSON Method (10× better than text-only)

1. **Find a reference image on Pinterest** — search for the aesthetic you want, save the image that matches your style
2. **Convert to JSON** — upload the image to Gemini 3.0 Pro and ask: "Create a detailed JSON to recreate this image." Copy the JSON output.
3. **Use JSON as base for your image** — paste the JSON + add your prompt. Example: `"Use this JSON as reference: [JSON] Now generate: woman holding my product on beach."` Upload your product image if needed.
4. **Generate in AI.Studio** (best platform for Nano Banana Pro) — paste your JSON + prompt and generate.

**Why this works:** The JSON handles realism + color grading. Your prompt handles the actual content. This prevents the standard grey AI look that comes from text-only prompts.

**Alternative method:** Find reference content on TikTok in your niche. Screenshot a good frame, put it into Claude or Gemini, ask it to create a JSON prompt to recreate that image. Then adjust the prompt for your own unique character — different clothes, different background. Your starting frames will already look like content that performs in that niche.

## Character Preservation (Background Swap)

To change the background while keeping a character identical, lead with:

```
[reference person 1] — preserve all facial features, hair color and style,
skin tone, body type, and physical characteristics of the reference photo 1
exactly as they appear
```

Then describe the new scene/background after this line.

## Best Practices

- **Always generate in 4K** — these are start frames for video, resolution matters
- **Match aspect ratio to platform** — decide this before generating, not after
- **One subject per frame** — keep it clean for Kling/Seedance to interpret
- **Nail the appearance here** — video prompts should only handle motion, dialogue, and tone
- **Text on products** — use "perfect text" and "legible text" modifiers. Works well in stills; video models will smudge small text regardless (model limitation)
- **Lighting in the image sets the mood** — Kling/Seedance will largely inherit the lighting from the start frame

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

## Content Type Notes

### AI Podcast / Talking Head
- Direct-to-camera framing, eye contact with lens
- Clean background (solid, blurred, or simple set)
- Good for Kling 3.0 simple prompt structure

### UGC Product Review
- Subject holding or interacting with product
- Casual setting (couch, desk, kitchen)
- Use "perfect text" modifier if product has visible text/branding

### Military / Action
- Accurate gear matters — get tactical equipment right in the image since video models won't
- Reference: correct NODs (GPNVGs), plate carriers, weapon systems, IR devices
- Seedance 2.0 will handle the motion; NBP handles the gear accuracy

## See Also

- [[Skills/kling-video/SKILL|Kling 3.0 Prompting]]
- [[Skills/seedanceX/notes|Seedance 2.0 Prompting]]
- [[Skills/NBPHero/notes|NBPHero — product hero specialist]]
