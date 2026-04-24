---
type: skill-reference
skill: NBPHero
topic: kit-arrangements
---

# Locked-In Product Kit Arrangements

User-editable library of multi-product flat-lay arrangements. Each entry specifies which product goes where, relative sizing, backdrop, and lighting — so renders are reproducible.

## Schema

```
### [Kit name] flat-lay ([N products])

- Image 1: [Product A] → position [role, e.g., "anchor, largest, center"]
- Image 2: [Product B] → position [role]
- ...

Sizing notes: [which product anchors; relative scale of others]
Backdrop: [material + color]
Lighting: [named source + direction]
Aspect: [1:1 typical for flat-lay]
```

## Library

_Empty. Populate as Yasir builds kits._

### Example template (replace with real entries)

```
### [Example] 5-product flat-lay

- Image 1: [Product A bottle] → products 1 and 2 (identical, placed twice, upper-left and upper-right)
- Image 2: [Product B accessory] → product 3, lower-left
- Image 3: [Product C accessory] → product 4, lower-right
- Image 4: [Product D pouch] → product 5, anchor position in center, largest

Sizing notes: pouch is anchor; bottles, accessories all fit inside.
Backdrop: neutral linen tablecloth, cream.
Lighting: soft overhead studio lighting, slight warm tilt.
Aspect: 1:1 square framing.
```

## Usage

When NBPHero is asked to render a kit, it checks this library first. If found, use the arrangement verbatim. If not found, ask the user to describe the arrangement, save here, then render.
