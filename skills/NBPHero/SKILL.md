---
name: NBPHero
description: Use when the user asks to generate a product hero shot, render a product label, transcribe a label onto a bottle/box/jar/package, build a flat-lay kit of multiple products, or mentions "NBPHero", "product hero", "label render", "wordmark hero", "hero wordmark", or "locked-in product kit". Specialist child of `nbp` for product-still work where text fidelity matters.
version: 1.0.0
tested_on: Nano Banana Pro via AI.Studio (April 2026)
---

# NBPHero — Product Hero Image Specialist

NBP specialist for product-still work: label transcription, product type descriptions, locked-in kit arrangements, text-rendering reality checks with a compositing fallback.

## When to Use

Activate when the user:
- Asks for a "product hero shot", "hero wordmark", "label render", "product kit flat-lay"
- Needs readable text on a product (bottle, box, tube, jar, bag, package)
- Wants a multi-product flat-lay with locked-in arrangement
- Says "/NBPHero" or "/nbphero"

Do NOT activate for:
- Character/lifestyle stills → parent skill `nbp`
- Video generation → `seedanceX` or `kling-video`

## Core Principle — The Text-Rendering Reality Check

NBP's text rendering has a ceiling. At 1:1 flat-lay scale with 5 products, each product gets ~15-20% of frame. **Large hero text renders cleanly. Secondary copy below ~8-12px per line often garbles.**

The 3-reroll rule: if secondary copy garbles after 3 re-rolls, composite the real label in Affinity/Photoshop — 10-minute fix per product.

## How to Use

### Step 1 — Check the label transcription library

Is there a saved label transcription for the target product in `Skills/NBPHero/label-library.md`? If yes, use it verbatim in the prompt. If no, ask the user to provide the label copy (line-by-line with hierarchy notes).

### Step 2 — Check the product type description library

Is there a saved product-type description (e.g., "quilted zipper pouch", "silk sleep eye mask")? If yes, use verbatim. If no, ask for a compact description that captures texture, shape, key visual elements.

### Step 3 — Lock the aspect ratio

| Use case | Aspect |
|---|---|
| Single product hero (e-commerce) | 1:1 square framing |
| Single product hero (ads) | 4:5 portrait or 1:1 |
| Flat-lay kit | 1:1 square framing |
| Hero with environment | 16:9 widescreen |

### Step 4 — Build the prompt

Assembly order (inherits from `nbp` parent):
```
[Product subject with saved type description] +
[Label transcription with hierarchy notes] +
[perfect text, legible text modifiers] +
[Environment / backdrop] +
[Lighting recipe — name source + direction] +
[aspect framing] +
[product photography style, 4K]
```

### Step 5 — Generate, then apply the 3-reroll rule

1. Generate at target aspect
2. Check secondary copy for garbling
3. If garbled: up to 3 rerolls with minor prompt tweaks
4. If still garbled after 3: fall back to Affinity/Photoshop composite (10-minute manual fix per product)
5. Always save successful label transcriptions back to `label-library.md` for reuse

### Step 6 — For locked-in kit arrangements

If the user has a documented kit arrangement (e.g., "5-product flat-lay with the pouch as anchor"), honor the arrangement notes exactly. See `kit-arrangements.md` for stored arrangements.

## The Hero Prompt Template

```
[Product type description], cream label with "[HERO WORDMARK]" in [color]
as the hero text, [secondary line with hierarchy], [tertiary line], perfect
text, legible text, on a [backdrop description], [named light source from
direction], shallow depth of field, 1:1 square framing, product photography
style, 4K
```

## Kit Flat-Lay Template

```
Flat-lay of [N products] on [backdrop material + color]. Products arranged:
[Product A, largest, anchor position], [Product B, upper-left], [Product C,
upper-right], [Product D, lower-left], [Product E, lower-right]. Each product
labeled as saved. [Named light source — typically "soft overhead studio
lighting"]. perfect text, legible text, 1:1 square framing, product
photography style, 4K
```

## Text-Rendering Ceiling (verbatim guidance)

At 1:1 flat-lay scale with 5 products, each bottle gets ~15-20% of frame. Large hero text (e.g., a prominent wordmark) renders cleanly. Secondary copy below gets ~8-12 pixels per line and often garbles — especially text adjacent to graphic badges. The 3-reroll rule plus Affinity/Photoshop composite is the operational workaround.

## Reference Files

Living in `Skills/NBPHero/`:
- `notes.md` — full reference with schema for saved transcriptions
- `label-library.md` — user's saved label transcriptions (empty initial template — populated over time)
- `product-types.md` — saved product-type descriptions (empty initial template)
- `kit-arrangements.md` — locked-in multi-product arrangements (empty initial template)

## See also

- `nbp` — parent skill for non-product NBP work
- `seedanceX` — downstream consumer if the hero shot is animated
- `kling-video` — downstream consumer for I2V product animation with orbit / turntable
