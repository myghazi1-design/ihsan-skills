---
type: skill-reference
skill: NBPHero
domain: video-production
tags: [nbp, product-hero, label-rendering, image-generation]
---

# NBPHero — Product Hero & Label Specialist

Specialist subclass of `nbp`. Handles product hero shots, label transcription, multi-product kit flat-lays, and the text-rendering ceiling workarounds.

## The Core Problem This Skill Solves

NBP produces 4K images that are great for people, scenes, and single products. But when you need:
- A readable wordmark on a product
- Multiple products in a flat-lay with each legible
- A kit arrangement that's identical render-to-render
- Brand-critical text fidelity

…you hit NBP's text-rendering ceiling. This skill encodes the workarounds: saved transcriptions, locked arrangements, and the 3-reroll-then-composite fallback.

## Saved Label Transcriptions

Structure for capturing a label so it's reproducible across renders. Kept in `label-library.md`.

### Schema

```
### [Product name] — [product type]

```
"[hero wordmark]" in [color/position], then "[secondary line]" [hierarchy note],
then "[tertiary line]" [hierarchy note], ...

Below that, a [graphic element description]. To the right of that, [additional
element] — render the [element A] and the [element B] as two separate elements
with visible clear space between them.
```

Reference image: [[path/to/product-reference.png]]
```

## Saved Product Type Descriptions

Compact descriptions that capture the material / texture / shape. Kept in `product-types.md`.

### Schema

```
### [Product name] — [brief type]
*"[one-line description: material, shape, key texture, branding accent]"*
Reference image: [[path/to/product-reference.png]]
```

Example format (generic, no actual product data):
```
### [Product X] — Quilted zipper pouch
*"quilted zipper pouch — match the puffy pillow-quilted texture, grid stitching,
metal zipper, and small woven label tag shown in the reference exactly"*
Reference image: [[path/to/reference.png]]
```

## Locked-In Product Kit Arrangements

For multi-product flat-lays, document the arrangement explicitly so renders are reproducible. Kept in `kit-arrangements.md`.

### Schema

```
### [Kit name] flat-lay ([N products])

- Image 1: [Product A] → position [A-role, e.g., "anchor, largest, center"]
- Image 2: [Product B] → position [B-role]
- ...

Sizing notes: [which product anchors; relative scale]
Backdrop: [material + color]
Lighting: [named source + direction]
```

## Text-Rendering Ceiling

At 1:1 flat-lay scale with 5 products, each item gets ~15-20% of frame. Implications:

- **Large hero text** (the dominant wordmark) renders cleanly
- **Secondary copy** at ~8-12 pixels per line often garbles, especially text adjacent to graphic badges (e.g., a numeric count on a circular badge next to a product descriptor)
- **Very small text** (ingredients, disclaimers) garbles nearly always

## The 3-Reroll Rule

1. Generate the hero image at target aspect
2. Check every text element for garbling
3. If any garbles: reroll with minor prompt tweaks (e.g., remove a modifier, tighten the label phrasing)
4. After 3 rerolls, STOP rerolling — move to composite
5. Composite the real label in Affinity/Photoshop — 10-min fix per product
6. If the reroll succeeded: save the final prompt + any changes back to `label-library.md`

## Pipeline Position

1. User provides product label copy and type description
2. NBPHero saves (or retrieves) them in `label-library.md` / `product-types.md`
3. NBPHero generates the hero image
4. If text garbles → 3 rerolls → composite fallback
5. Deliverable: 4K PNG hero image ready for ads, e-commerce, or as a `@Image2` product asset for Seedance/Kling UGC

## See also

- [[Skills/nbp/notes|Nano Banana Pro Prompting]] (parent)
- [[Skills/seedanceX/notes|Seedance 2.0 Prompting]] (downstream if animated)
- [[Skills/kling-video/SKILL|Kling 3.0 Prompting]] (downstream for I2V product orbit)
