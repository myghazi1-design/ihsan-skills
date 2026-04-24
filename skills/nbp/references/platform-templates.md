---
type: skill-reference
skill: nbp
topic: platform-templates
---

# NBP Platform-Specific Templates

Pre-filled templates by target platform. Swap in the bracketed sections.

## TikTok / YouTube Shorts / Instagram Reels (9:16)

```
[subject description], [specific details], [modifiers], 9:16 vertical framing, [style], 4K
```

### Example fills

**UGC talking head (9:16)**
```
A woman in her late 20s, casual t-shirt, wavy brown hair, sitting on a
couch in a bright living room, direct eye contact with camera, natural
window light, 9:16 vertical framing, photorealistic, 4K
```

**Product demo close-up (9:16)**
```
A pill bottle on a clean marble countertop, label facing camera with
"daily glow" wordmark visible, perfect text, soft morning light from
the left, shallow depth of field, 9:16 vertical framing, product
photography style, 4K
```

## YouTube Longform (16:9)

```
[subject description], [specific details], [modifiers], 16:9 widescreen framing, [style], 4K
```

### Example fills

**AI podcast setup (16:9)**
```
Two people in a minimal podcast studio, sitting across a wooden desk,
both wearing neutral colors, black microphones on articulated arms, soft
key light from the left and right, cinematic warm tones, 16:9 widescreen
framing, photorealistic, 4K
```

**B-roll establishing shot (16:9)**
```
A desert highway at dawn, long empty road receding to mountains, warm
golden-hour sunlight, slight mist over the horizon, film grain,
cinematic, 16:9 widescreen framing, 4K
```

## Instagram Feed (1:1)

```
[subject description], [specific details], [modifiers], 1:1 square framing, [style], 4K
```

### Example fills

**Product hero on colored backdrop (1:1)**
```
A single pill bottle centered on a deep teal backdrop, soft studio
lighting from upper left, "daily glow" wordmark legible on label,
perfect text, highly detailed, 1:1 square framing, product photography
style, 4K
```

## Instagram Portrait (4:5)

```
[subject description], [specific details], [modifiers], 4:5 portrait framing, [style], 4K
```

### Example fills

**Editorial portrait (4:5)**
```
A woman in her 40s, angular features, ochre linen blazer over white
tee, standing against a neutral grey studio backdrop, soft overhead
studio lighting, shallow depth of field, editorial fashion style,
natural skin texture, 4:5 portrait framing, 4K
```

## Cinematic Ultrawide (21:9)

```
[subject description], [specific details], [modifiers], 21:9 anamorphic framing, [style], 4K
```

Rare — only for cinematic B-roll or title cards. Seedance/Kling will crop.

## Aspect-Ratio Decision Rules

1. Lock aspect BEFORE generating — changing it later degrades quality
2. Match the final platform, not the intermediate model
3. If outputting for multiple platforms, generate separate NBP renders per aspect
4. Don't try to crop a 16:9 image to 9:16 — regen at 9:16 from the start
